# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This repo is a personal blog. The code is just Hugo + a vendored theme — it basically doesn't matter. **What matters is the writing voice.** Almost every task in here will be "help me draft/edit a post," and the whole point of this file is to make sure future Claudes don't flatten the voice into generic AI prose.

Read the existing posts in `content/posts/` before drafting anything. The voice notes below are a summary, not a replacement.

---

## The voice, in one paragraph

First-person, conversational, a little self-deprecating, Gen-Z cadence without being performative about it. Short paragraphs. Sentence fragments for punch. Reads like a guy thinking out loud in a group chat, not a writer "crafting a piece." Opinionated — he'll say "i hate it" or "that is not how it works" and commit. Swears when the feeling calls for it ("fucking", "shit", "bullshit"), never for decoration. Honest about his own gaps and doubts ("i might be wrong about some of it. probably am."). Ends most posts with a `:)` or `:))`.

## Voice reference: Michael Reeves

The author's writing voice leans toward Michael Reeves — the YouTuber known for comedy-tech videos, chaotic robotics builds, and deadpan delivery. He doesn't fully write like Reeves, but a chunk of the personality is adjacent, and it's a useful reference point when you're trying to calibrate tone.

What to actually pull from Reeves when drafting or editing:

- **Deadpan delivery over hype.** Reeves states absurd things flatly. The author does the same thing in prose — `"that is not how it works."`, `"wild concept, I know."`, `"which i know is probably impossible at this point."` The humor comes from flatness, not exclamation marks. If you're adding `!` to make something land, you're doing the opposite of what this voice does.
- **No facade of mastery.** Reeves openly yells at code that doesn't work and mocks himself when a build fails. The author does the written version of this: `"i'll be honest, some AI had to be used to understand certain parts of this"`, `"i'm just a guy trying to make cool things work"` energy. Never let the prose pretend he has everything figured out. Competence is shown through specific technical detail, not through confident framing.
- **Unhinged-but-technical.** The mix is the point. The go-wasm post is the clearest example: real engineering content (syscall/js, Promise wrapping, wasm_exec.js env hack), but delivered with `"which sounds cursed. and it kind of is"` alongside the code. When editing a technical post, keep both halves — don't sanitize the unhinged side into neutral docs prose, and don't strip the engineering detail in the name of "making it more casual."
- **Edge without cruelty.** Reeves is described as an "edge lord" but the humor punches at himself and at abstract stuff (AI hype, tech CEOs, his own life choices), not at real people. Keep that. Profanity and sharp takes are fine. Don't write mean things about named individuals the author hasn't already dragged himself.
- **Absurdity as structure.** Reeves builds a serious robot to solve a fake problem. The laptop post works on the same skeleton — a real conversation about AI framed around the absurd detail of a stranger abandoning his laptop at the table. If you're helping structure a post, look for the absurd anchor and build around it.

What to **not** pull from Reeves:

- Don't impersonate him. Don't drop YouTuber-isms the author has never used. No `"what's up gamers"`, no screaming into the text, no direct camera asides.
- He's a reference for energy and cadence, not a costume. If in doubt, match the existing posts in `content/posts/` — they're the actual ground truth. Reeves is just the north star when the posts are ambiguous about tone.

## Capitalization — there are two modes, don't mix them

There's been a style drift across the posts. Both are valid; pick one per post and stay consistent.

- **lowercase mode** (posts before `2026-03-29`): everything lowercase, including `##` headings and the front-matter `title`. Sentence-initial letters dropped. `i`, not `I`. This is the default for short, thought-dump posts.
- **Normal Case mode** (from `2026-03-29` onward, `the-guy-who-left-his-laptop`): Title Case for headings, sentence case for prose (so `I`, `It`, etc. are capitalized), but the tone stays just as casual. This is used for longer, more narrative posts.

When continuing an existing post, match its existing mode. When starting a new one, ask or match the most recent post's mode by default.

## Paragraphs, rhythm, and punch

- Keep paragraphs short. One-to-three sentences is typical. A single-sentence paragraph is a valid move and he uses it for emphasis.
- **Sentence fragments are load-bearing.** "no." "it never ends though." "wild concept, I know." Don't "fix" them into complete sentences.
- Use the "setup, beat, payoff" rhythm for stories: establish the scene in plain sentences, then drop a short one-liner as the turn. See the laptop post: *"Except as he was walking away, he glanced at my screen. And there it was. Claude Code. Running with dangerously-skip-permissions. In all its glory."*
- Em dashes show up, but sparingly. Don't carpet-bomb posts with them — that's an AI tell.
- Avoid the rule-of-three pattern ("fast, cheap, and reliable"). That's another AI tell. He uses pairs and ones more than triples.

## Openers and closers

- **Openers** are conversational, not thesis-statement-y. Templates he actually uses: `"yo, so..."`, `"so i built a url shortener."`, `"have you ever actually sat down and..."`, `"It's been a minute. But I'm back..."`. Never `"In this post, I'll explore..."` or `"Today we're going to talk about..."`.
- **Closers** are short, often a tiny self-aware coda, and usually end with `:)` or `:))`. Examples: `"that's the whole point anyway."`, `"that's the part i'm still working on. :)"`, `"no pressure. no cap. let's go :))"`. Don't write summary paragraphs that restate the post's points.

## Structure

- `##` headings are used for sections, and the heading itself is usually casual or punchy, not descriptive (`"the old days"`, `"The Flood"`, `"AGI and Other Fairy Tales"`, `"the async nightmare"`). Not `"Background and Motivation"`.
- Short posts (blog-init style) skip headings entirely. Don't force sections onto a thought-dump.
- Technical posts (go-wasm style) still use the casual voice inside code-heavy sections, and include real code blocks with a language tag (` ```go `, ` ```js `). Inline code in backticks is fine.
- Asides in parentheses are common and welcome: `"(obviously you should always test, i'm just showing how powerful these tools have become.)"`

## Recurring themes and opinions to stay consistent with

These aren't rules, they're his actual positions. Don't contradict them without explicit instruction:

- AI/LLM tools make him unbelievably productive and he hates how that feels. He pays for Claude Max. He uses it every day. He wants to use it less. This tension is the core theme of multiple posts and should not be softened.
- "Vibe coding" is a term he uses critically. It's a multiplier on existing skill, not a leveler. Business owners can't ship post-MVP without a real technical lead.
- AGI hype is a hype cycle. The layoffs are mostly over-hiring correction with AI as a convenient excuse. He'll say this plainly.
- primeagen, theo, and Claude Code are recurring reference points. Don't swap them for other creators/tools unless he says so.
- He calls himself an undergrad. His readership is small and he jokes about it ("my three readers", "collect dust for a bit").
- He's learning Go and Rust on the side. Still finds Rust's `Result` handling rough. Don't write him as a Rust expert.

## Words and phrases — use / don't use

**Use freely** (these are all attested in posts): `yo`, `so`, `basically`, `honestly`, `literally`, `obviously`, `man`, `bro`, `my guy`, `lol`, `haha`, `no cap`, `boom`, `the thing is`, `here's the thing`, `that's just how it is`, `i mean`, `i'll be honest`, `wild concept, I know`.

**Never use** (generic AI tells — he doesn't write like this and the whole point of this file is to keep you from drifting there):
- `delve`, `dive into`, `deep dive`, `unpack`, `unlock`, `leverage`, `utilize`, `harness`, `streamline`, `robust`, `seamless`, `cutting-edge`, `game-changer`, `paradigm`, `ecosystem` (as a buzzword)
- `it's worth noting that`, `it's important to note`, `that said`, `furthermore`, `moreover`, `in conclusion`, `to summarize`, `in this article we will`
- `navigate the complexities of`, `in today's fast-paced world`, `a journey`, `a deep dive into`
- Bulleted "key takeaways" sections at the end of posts. He doesn't do summaries.
- Rule-of-three triplets used as filler. If you catch yourself writing "X, Y, and Z" where Z is padding, cut Z.

## Profanity

He swears. It's part of the voice. Don't add swears that aren't there, but also don't scrub swears he'd naturally use. `"you really fucking can't, my guy"` is correct; sanitizing that to `"you really cannot, my friend"` kills the post.

## Honesty and hedging

He hedges by admitting limits, not by softening claims. Good hedge: `"i might be wrong about some of it. probably am."` Bad hedge: `"some might argue that there are multiple perspectives on this issue."` If a claim is strong, keep it strong, then acknowledge uncertainty in his voice if needed.

## When he asks Claude to "structure" a post

He has done this before (see the vibe-coding-paradox post: *"i asked claude to structure this properly so it's actually readable"*). "Structure" for him means:
- Keep every sentence of his actual thoughts, in his words.
- Reorder paragraphs into sections with `##` headings.
- Add heading titles in his voice (lowercase or Title Case matching the post's mode).
- Do **not** rewrite sentences into neutral prose. Do **not** add transitions he didn't write. Do **not** insert an intro or conclusion that sounds like a blog template.
- If something is genuinely unclear, ask before rewriting it.

## Front matter and filenames

- Filenames: `content/posts/YYYY-MM-DD-kebab-slug.md`.
- Front matter: TOML (`+++`) in newer posts, YAML (`---`) in older ones. Match the format of recent posts (`+++`) for new work unless otherwise told.
- New posts default to `draft = true`. Only flip to `false` when he explicitly says to publish.
- Date uses `+05:30` offset on the posts that have a time component.

## Hugo mechanics (only if you actually need them)

- `hugo server -D` — preview with drafts.
- `hugo` — build to `public/`.
- `hugo new content posts/YYYY-MM-DD-slug.md` — scaffold from `archetypes/default.md`.
- Theme `nostyleplease` is vendored under `themes/` (not a submodule). Prefer creating overrides under repo-root `layouts/` or `assets/` over editing vendored theme files. There's no lint, no tests, no CI. Publishing is editing markdown and pushing.
