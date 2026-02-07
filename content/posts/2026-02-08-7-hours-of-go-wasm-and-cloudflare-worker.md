---
title: "7 hours of go, wasm, and cloudflare workers"
date: 2026-02-08T00:00:00+00:00
---

so i built a url shortener. go, compiled to webassembly, running on cloudflare workers. sounds straightforward when you say it like that. it was not.

## the backstory

i've been learning go on the side for a while now, and i wanted to actually build something with it on cloudflare. the thing is, my usual workflow is just dockerize whatever i'm working on and throw it into cloudflare containers. easy, works, done.

but a while back i did [this load test comparison](https://www.reddit.com/r/CloudFlare/comments/1pbdmc6/hono_vs_golang_on_cloudflare_workers_load_test/) of hono vs go on cloudflare workers and got some heat because i used containers instead of wasm. and honestly, fair point. so this time i wanted to do it properly — actual webassembly, no containers.

## what i was building

the concept is dead simple. POST a url, get back a shortened link. hit that short link, get redirected to the original. that's literally it.

for storage i went with cloudflare workers KV over D1. for a url shortener you're doing pure key-value lookups — give me the url for this id, store this id pointing to this url. KV is built for exactly that. D1 would've been overkill.

for generating the short ids, i needed something that would give me a consistent, short output for any url. looked into proper hash functions but they were all way too long for a url shortener. ended up going with CRC32 — it's technically a checksum algorithm, not a hash, but for this use case it works great. same url always produces the same id, the output is short enough to be useful, and for a personal url shortener the collision probability is basically irrelevant.

## getting into wasm

this is where things got interesting.

compiling go to wasm is straightforward. `GOOS=js GOARCH=wasm go build` and you get a `.wasm` file. cool. i read through cloudflare's announcement post on wasm support, then their docs, and ended up on this whole side quest about SIMD (single instruction, multiple data) since cloudflare mentioned they support it. that was a nice rabbit hole.

but getting the standalone wasm working and getting it integrated into a cloudflare worker are two completely different problems.

## wasm_exec.js

go provides this file called `wasm_exec.js`. it's the runtime glue that lets your go wasm binary actually run in a javascript environment. you create a `Go` instance, pass it your wasm module, call `go.run()`, and your go code starts executing.

it works. but it has no typescript types. at all. everything is `any`, everything is `@ts-ignore`. someone really needs to make a typed version of this. might do it myself at some point, who knows.

## the env variable thing

first real problem: environment variables. the go code needs cloudflare credentials to talk to the KV API. in a normal go program you'd just use `os.Getenv()`. in wasm running inside a cloudflare worker, there is no OS.

turns out `wasm_exec.js` has this `env` property on the Go constructor that seemed like it wasn't really used for anything? but when you pass key-value pairs into it at initialization, they become available through `os.Getenv()` on the go side. so in the typescript entrypoint i'm doing:

```js
const go = new globalThis.Go({
  CLOUDFLARE_ACCOUNT_ID: env.CLOUDFLARE_ACCOUNT_ID,
  CLOUDFLARE_NAMESPACE_ID: env.CLOUDFLARE_NAMESPACE_ID,
  CLOUDFLARE_API_TOKEN: env.CLOUDFLARE_API_TOKEN,
});
```

and on the go side, `os.Getenv("CLOUDFLARE_API_TOKEN")` just works. took a while to figure that out though.

## syscall/js and the two-function problem

so in go, `syscall/js` is the package that lets your wasm code interact with javascript. you can register go functions on `globalThis` so javascript can call them. my `main()` function registers two functions and then blocks forever with `select {}`:

```go
func main() {
    js.Global().Set("createShortUrl", js.FuncOf(createShortUrl))
    js.Global().Set("getRedirectUrl", js.FuncOf(getRedirectUrl))
    select {}
}
```

the problem is how you start this. normally you'd do `await go.run(instance)` and your go code runs. but `go.run()` with that `select {}` never resolves — it blocks forever, which is the whole point (keeps the go runtime alive so the functions stay registered). if you await it, your worker hangs.

the solution: don't await `go.run()`. just call it and let it start the go environment in the background. by the time you need to call `createShortUrl` or `getRedirectUrl`, the go runtime is already up and the functions are registered on `globalThis`.

## the async nightmare

this was the worst part.

each go function needs to make HTTP calls to cloudflare's KV API. those are async operations. but `syscall/js` function callbacks are synchronous — you return a value, and that's what javascript gets.

so how do you return something that hasn't happened yet?

you return a javascript Promise from go. which sounds cursed. and it kind of is. you grab `Promise` from the global scope, create a new one with a handler function, and inside that handler you spawn a goroutine that does the actual async work and calls `resolve` or `reject` when it's done:

```go
func createShortUrl(this js.Value, args []js.Value) any {
    promise := js.Global().Get("Promise")
    handler := js.FuncOf(func(this js.Value, args []js.Value) interface{} {
        resolve := args[0]
        reject := args[1]
        go func() {
            // do the KV API call
            // resolve.Invoke(result) or reject.Invoke(err)
        }()
        return nil
    })
    return promise.New(handler)
}
```

this works. the typescript side can just `await globalThis.createShortUrl(url)` and get back the checksum. but getting here involved a lot of staring at error messages about the event loop not being happy about async operations in a sync context.

the core issue was that go's goroutines are doing async HTTP calls, but the cloudflare workers runtime expects things to flow through its own async model. returning a proper JS Promise from the go side was what finally made it all click together.

## the actual flow

so the final architecture looks like this:

1. request hits the cloudflare worker (typescript)
2. typescript instantiates the wasm module, starts the go runtime
3. for POST requests: validate auth, parse the url from the body, call `globalThis.createShortUrl(url)`, get back a CRC32 checksum, return the short url
4. for GET requests: grab the id from the path, call `globalThis.getRedirectUrl(id)`, 301 redirect to the original url

simple once it's all wired up. getting it wired up was the hard part.

## the resources problem

one thing that made this harder than it needed to be: the resources for go + wasm + cloudflare workers are really scattered. cloudflare has some docs. go has some docs. the intersection of the two is mostly github issues and random blog posts from people who've tried it. a lot of the gotchas i ran into aren't documented anywhere obvious.

i'll be honest, some AI had to be used to understand certain parts of this. the `syscall/js` API specifically — the documentation for it is minimal and the patterns for doing async work through it aren't intuitive at all. but at least i understand what's happening now, which is the part that matters to me.

## was it worth it

i mean, it works. it's deployed. it shortens urls. took about 7 hours from start to finish, most of which was spent on the integration layer between go wasm and the cloudflare workers runtime.

would it have been faster to just write the whole thing in typescript? absolutely. like, 30 minutes faster. but that wasn't the point. i wanted to use go on cloudflare workers properly this time, through wasm, and understand how all the pieces fit together.

and now i do. mostly. the code isn't the cleanest thing i've ever written — the types are a mess, there are `@ts-ignore` comments everywhere, and the go-side promise construction is pretty ugly. but it ships, it's fast, and i learned a lot building it.

that's the whole point anyway.
