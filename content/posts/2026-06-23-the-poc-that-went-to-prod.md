+++
title = 'the poc that went to prod'
date = 2026-06-23T00:00:00+05:30
draft = false
+++

This is probably my longest one yet. It's also my rantiest. So get ready lmao.

I'll be honest with you up front, this whole thing has a lot of weird ups and downs. That's just how the project went. So that's how the post is gonna go too.

## The Back Burner

So there's this project that's been sitting in the back of my mind for, what, months? It was being worked on by another team member. A friend of mine. They took the initiative on it and I genuinely respect them for that. But it had constant bugs. Constantly.

The situation was a bit awkward. We were working with a set of middle men, and through them we'd sort of work with the client. So we were never talking to the people who actually wanted the thing. We were talking to the people in between.

And it was good. It was fine. Sorta.

The main problem from the very start was that communication from the middle man side was shit. Just shit. So we had to fend for ourselves most of the time.

## It Literally Says PoC in the Repo Name

Here's the funniest part.

I did the initial proof of concept. I built it. And the repo name literally has "PoC" in it. Proof of concept. It was never supposed to go off like this. It was never meant to go to production.

But it did.

And as the guy in the PM-ish role, I regret that I let it happen. I blame myself. Because I just took it, handed the whole responsibility over to my friend, and called it a day. Never checked up on it. Not once.

That's a dummy 101 mistake. The most basic thing you're not supposed to do. And I did it. I regret that a lot.

## Nobody Wanted to Touch It

So here we are. We'd promised these people we'd fix the entire thing countless times. Countless. But it was such a mess that nobody wanted to take it up.

And honestly? Even I wouldn't have wanted to.

## The Guy at Work

Then I was talking to a friend from work. He was telling me about the project he works on. That's all he works on, just that one thing. And he's built up this... power, sort of. To the point where if he doesn't want something to happen, it doesn't happen. How? I don't fully know. Technical dominance, domain dominance, something like that.

But the point is, I watched him take the most advantage of the position he'd been put in. And that genuinely inspired me.

That night, right after the conversation, I was like fuck it. I'm gonna do this. I'm gonna refactor and fix this fucking project up.

So that's the start of the refactor. The project that, by the way, is still ongoing. I wanted closure from it. To finally call it a day. And I still couldn't, not properly. There are a few bugs left, but those are mostly a middle man issue at this point. Either way I'm not letting this post sit in my to-do list any longer lmao.

So let me take you through it. Three major refactors. A full database schema migration. The fact that our schema hadn't been updated in three months. And how we ended up on Hono and React when we started on Next.

53 commits over about a month. Somewhere north of 100k lines added, 134k deleted. 327 files just straight up gone, 230 of them TypeScript. We deleted way more than we wrote, which tells you basically everything about what was already in there.

## The Codebase Was a Gonner

Before we get into any of it, let me tell you what this codebase looked like.

Comments everywhere. Circular function calls, one nested inside another. Dead code everywhere. I had genuinely not seen a codebase like this. Or if I had, it's not one I'd wanna be caught dead in.

A few days before the refactor I tried to troubleshoot an issue that got raised. And holy shit. There was no understanding this code. It was just a gonner. Done.

We actually considered dropping the entire thing and moving to a whole new system. But I didn't want to. That would've been the biggest migration we'd ever done. A full refactor is hard enough, now imagine doing it across systems. I didn't want that.

Or maybe I just wanted the challenge? I don't know. Maybe I was inspired by my friend. Bro was working on microservices-level stuff, and here's my ass on this fuck ass thing.

But it's all about appreciating the position you're in. When I got started programming a few years back, I would've killed to be where I am now. Working on an actual "production" application. I can't tell you how hyped I would've been. And somewhere along the way I'd taken that for granted.

That ended up being a weird motivator through all of this. It was for myself. :))

Oh, and there was this whole thing where we'd shipped a really, really shitty update a few days before. The one I was troubleshooting. The middle men weren't happy about it, understandably. So them being rightfully bitchy about it was part of the motivation too lol.

## Starting by Hand

I started off by literally going through the code. By hand. Just trying to understand what the hell was going on.

There was so much random unwanted low-key shit in the root dir. Random scripts. So much of everything all at once. But not a functioning system. That part was clear.

Then I started on some basic perf updates, since some pages were taking, what, two minutes to load? Minutes. To load a page. Crazy. I moved our databases, prod and dev, into the Mumbai region. We're on Cloudflare infra, and they'd been sitting all the way in Europe.

Then I deleted the useless files and set up knip, lefthook, and biome. Basic stuff. Subtle. But this was one of the best things I did, honestly. The pre-commit and pre-push checks, all of it, so worth it. And when I ran knip, the dead code analyzer, the count was in the hundreds. Hundreds.

## CLAUDE.md Did the Heavy Lifting

This was probably the single biggest thing I integrated.

The existing CLAUDE.md was mid as hell. So I updated it as we went. And I got it to a point where it was basically leading the workflow. It would implement properly, ask me any questions it had, update and expand the test cases as needed, and use Claude, pi code and codex to verify the changes.

Subtle. But this sped up the development workflow like crazy.

## Server Actions to tRPC

The first big migration was moving off Server Actions onto tRPC v11. Mainly for type safety. Server actions just aren't as type safe as tRPC in terms of how end-to-end they are, so I went with that. Then we put tanstack-react-query on top of it.

It was a bit scary. But not as scary as what came next. This one was actually fairly easy, surprisingly. Just a lot of good prompting lol.

## The Database Migration That Scared the Shit Out of Me

This was the biggest scare so far. A full database migration. From the old, stale version.

And guess what. We hadn't updated our schema since I handed over the PoC. ;-; Somehow Claude never figured out how to do that the whole time? I genuinely don't understand how that happened. But because of it, the database was a mess. Empty columns. Absolute and utter mess.

I was honestly surprised the system even worked the way it was. It was this join, that join, joins on joins, a barely surviving fucking system. Crazy that it ran at all.

But the refactor got done. We rebuilt the schema basically from scratch. Global cycles, a worker ledger, camelCase tables, 28 fresh indexes. One of the scariest ones I've done. We even had to seed and generate some data, because some of it just wasn't there. Historical data and stuff like that. So scary. But hey, we made it.

And then, weeks later, I had to go back and drop 33 legacy duplicate indexes the rename had left lying around. The mess keeps on giving.

## A Test Suite, Finally

Then we brought in a test suite through vitest. One of the best decisions, honestly. Integration and e2e, covering all 151 tRPC endpoints.

This made me so much less scared to release to stable every single time. I felt like I could actually rely on it. The vitest coverage is just for the backend, we still don't have frontend-level tests since the frontend is ever-changing, at least for now.

We carried this over into the new Hono version too, and it's something that has to be maintained as we keep going. Does it eat some CI/CD time? Yeah. But it's all worth it.

After that we did some UI changes, and that was it. A good, stable, end-to-end type safe Next.js application.

## Wait, Why Are We Even on Next?

So then what happened? Why go Hono and React?

Well, I was sitting there thinking, are we even using the functionality Next gives us? Not really. ;-;

That was the trigger. I realized that Next, in this scenario, was just bloat we were shipping around for no reason. Waste of time. Waste of compute, honestly. So, hello there, major breaking refactor lol.

We haven't pushed this one to prod yet. But we're already seeing the performance increases.

## One Worker, Two Jobs

One thing worth mentioning about this new setup. Vite, React and Hono, all sitting in one turborepo monorepo, running in the same single Cloudflare worker. Which is an interesting approach, more interesting than what I've seen before at least.

What we do is run the Hono API in the worker, and render the SPA, the built version of the React app, in as the assets. So anything hitting `/api` goes to Hono. Everything else goes to the assets and React handles it.

It's a very odd thing. Never thought it was possible lol.

## How It Actually Worked

So how did all of this happen? How did this refactor end up successful, if you wanna call it that? Honestly, it came down to a few little things.

- **A GitHub Project to manage the bugs and feature requests.** Tiny thing. Subtle. But fuck me, this was a killer advantage. Any bug the middle men mentioned, we put it on there. And it actually got tracked and maintained.
- **GitHub Issues for everything.** And I specifically required my Claude to go and update those issues, add comments, leave context for later implementations. So useful.
- **Human review.** I set up a workflow where when a PR is in the draft state, I'd go review it, and the Claude instance would fix my comments, reply back, resolve them, and keep going.

Very subtle. But very good.

## What I Learned

- Don't half ass shit lmao.
- Appreciate the opportunity you're given.
- But for real, technically? I didn't learn much. It was just stuff I already knew existed, finally implemented. I never really coded any of it. It was entirely "agentic engineered" lol.

My AI setup for all of this was a Max 20x Claude subscription. Crazy, I know. Privileged ass. (It's a shared one, bud.)

## What's Next

- I just want this done, honestly. At this point it's random features and random shit the client wants, and I can't be asked bro.
- I want to try splitting the database into durable objects, to handle more concurrent requests, since D1 is single-threaded.
- And I might actually consider a Rust rewrite if it ends up more performant. Compiles to wasm lol.

That's the story. :))
