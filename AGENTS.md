# AGENTS.md

Guidance for coding/writing agents working in this repository. The full voice guide lives in [CLAUDE.md](./CLAUDE.md) — read it. This file is the short version.

## What this repo actually is

A personal blog. The Hugo setup is incidental. **The work is almost always about writing — drafting posts, editing posts, structuring a thought dump into sections.** Don't treat this like a software project where correctness is the goal. The goal is voice fidelity.

## The one rule

**Do not flatten the voice into neutral AI prose.** If you cannot tell whether a sentence you wrote sounds like the existing posts in `content/posts/`, stop and re-read two or three of them before continuing. The default failure mode here is polishing a casual post into something that sounds like a LinkedIn article.

## Voice north star: Michael Reeves

The author's personality is partly adjacent to Michael Reeves (YouTuber, comedy-tech, chaotic robotics, deadpan delivery). When tone is ambiguous, lean Reeves: deadpan over hype, no facade of mastery, unhinged-but-technical, absurdity used as structure, edge without cruelty. Full notes in [CLAUDE.md](./CLAUDE.md#voice-reference-michael-reeves). It's a reference, not a costume — the posts in `content/posts/` are still the ground truth.

## The five-second voice check

Before finalizing any sentence you wrote for a post, run it against these:

1. **Would he actually say this out loud to a friend?** If it only works on paper, rewrite it.
2. **Is there a rule-of-three triplet, an "it's worth noting," a "delve into," a "seamless," a "robust"?** Delete it. Those are AI tells. See CLAUDE.md for the full banned-words list.
3. **Is the paragraph over four sentences?** Consider breaking it. His paragraphs are short.
4. **Did you add a summary / "key takeaways" / "in conclusion" section?** Delete it. He doesn't write those.
5. **Does the post end with `:)` or `:))`?** Most of his do. Don't force one, but don't strip it either.

## Capitalization mode — pick one, stay consistent

Two modes exist across his posts:
- **all-lowercase** (pre-`2026-03-29`): headings and body.
- **Normal Case** (from `2026-03-29` onward): Title Case headings, sentence-case body.

Match the mode of the post you're editing. For a new post, match the most recent post or ask.

## Things to never do

- Never rewrite his sentences into "cleaner" prose when he asks you to *structure* a draft. Structuring = reordering + adding `##` headings in his voice. Nothing else. See the vibe-coding-paradox post for how he's used Claude for this before.
- Never scrub profanity. `fucking`, `shit`, `bullshit` are part of the voice when the feeling calls for it.
- Never soften his strong opinions (AI hype cycle, vibe coding critique, AGI skepticism, layoffs-are-over-hiring-correction). They're consistent across posts and they're the whole reason anyone would read this blog.
- Never invent facts, projects, or opinions he hasn't stated. If you don't know, ask.
- Never touch `public/` — it's Hugo build output and gets regenerated.
- Never change front matter from TOML (`+++`) to YAML (`---`) or vice versa inside an existing post. Match what's already there.
- Never publish a draft (`draft = false`) unless he explicitly says to.

## Things to do

- Read at least two existing posts before drafting anything new. Voice is absorbed, not described.
- When in doubt about tone on a specific line, default to "group chat message" not "published essay."
- Keep code blocks tagged with a language (` ```go `, ` ```js `, etc.) — he does this consistently in technical posts.
- Keep parenthetical asides when he uses them. They're part of the rhythm.
- Preserve his self-deprecating bits ("my three readers", "probably won't happen lol"). Don't "upgrade" the confidence level.

## Hugo-side ground rules

- New posts: `content/posts/YYYY-MM-DD-kebab-slug.md`, scaffolded via `hugo new content posts/...` so they inherit `archetypes/default.md`.
- Preview drafts with `hugo server -D`. Build with `hugo`. No tests, no lint, no CI — it's just markdown.
- Theme `nostyleplease` is vendored. If a visual change is needed, create an override under repo-root `layouts/` or `assets/` rather than editing `themes/nostyleplease/**`.
