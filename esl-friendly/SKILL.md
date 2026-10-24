---
name: esl-friendly
description: Rewrite English text for non-native speakers (CEFR B2) without losing technical precision. Detects idioms, phrasal verbs, metaphors, and non-literal phrases, replaces them with literal English, and (in learn mode, the default) appends a mini-glossary so the reader learns the original expressions. Use when the user says a response is hard to understand, asks to simplify or re-explain English, asks what an English phrase means, or wants responses written in plain learner English. Technical vocabulary, code, commands, and identifiers are never simplified.
---

# ESL-Friendly

Make English text understandable for a non-native speaker around CEFR B2 — **without dumbing down the technical content**.

The reader understands technical vocabulary. The problem is idioms: "half right", "where things stand", "the config dance", "the test is green". This skill replaces those with literal English, and teaches the originals in a small glossary.

## When to use

- The user says they don't understand a response, or part of it.
- The user asks to simplify, re-explain, or rewrite English.
- The user asks what an English word or phrase means ("what does 'worth flagging' mean?").
- The skill is loaded as a standing rule: then every response you write follows this style from the start.

## The golden rule

**Simplify the language, never the content.**

- NEVER touch technical vocabulary: identifiers, CLI flags, file paths, error messages, API fields, terms like *bind mount*, *schema*, *deploy*, *container*. The reader knows these.
- NEVER shorten, summarize, or drop information. This is a rewrite of the language, not a summary of the content. All facts, numbers, steps, and caveats stay.
- Code blocks, commands, paths, and values stay byte-identical.

## What to replace

Detect and rewrite, in order of importance:

1. **Idioms** — "half right" → *partially correct*; "where things stand" → *the current status*; "on the same page" → *agreeing*.
2. **Phrasal verbs with non-obvious meaning** — "pick up where I left off" → *continue from the previous point*; "figure out" → *understand / find the cause*; "boils down to" → *means, in essence*; "worth flagging" → *important to mention*.
3. **Metaphors** — "the config dance" → *the complicated config routine*; "it would hijack the session" → *it would take control of the session*; "two traps" → *two common mistakes*; "low-hanging fruit" → *the easiest tasks*.
4. **Non-literal words** — "the test is green" → *the test passed*; "this cost a debugging cycle" → *this caused one extra debugging cycle*.

Keep a literal word when it is standard technical usage, even if metaphorical in origin: *spawn*, *kill a process*, *boot*, *watch*, *daemon* are fine — a B2 technical reader meets them everywhere.

## What NOT to change

- Technical vocabulary and jargon (the reader's strength).
- Code, commands, file paths, config keys, error strings, exit codes.
- Sentence content: one rewritten sentence must carry exactly the same information as the original.
- The user's own words when quoting them.

## Modes

### `learn` (default)

Rewrite the text literally, then append a glossary of the expressions you replaced:

```
Idioms in this response
- "half right" — partially correct
- "worth flagging" — important to mention
- "hijack" — take control of something by force
```

Glossary rules:

- Order: same order the expressions appeared in the text.
- Explanation in simple literal English (B1–B2 level). This is the learning part: the reader keeps meeting the idiom with its meaning, in context.
- Add a hint in the reader's conversation language when it is not English, e.g. for a Spanish speaker: `- "half right" — partially correct (Spanish: a medias bien)`. Keep hints to a few words.
- Only expressions that actually appeared. Never pad.

### `simple`

Rewrite literally, no glossary. Use when the reader only wants to understand, not learn.

### Standing rule (always-on)

When loaded as a session rule, write your own responses directly in this style:

- Short sentences, active voice, one idea per sentence.
- Prefer literal verbs; if you must use an idiom, gloss it inline in parentheses the first time: "the test is green (passed)".
- No preamble, no filler.

## Rewrite quality rules

1. One sentence, one idea.
2. Active voice: name the agent of the action.
3. Short word over long: *use* over *utilize*, *start* over *initiate*. But keep the precise technical term when it is the correct one — never replace *idempotent* or *race condition*.
4. Keep the paragraph structure of the original; the reader maps the rewrite to the original position by position.
5. Never translate the whole response into the reader's language unless they ask — the goal is English they can read.

## Examples

**Original:**
> The first test is complete and green. Two traps worth knowing about: omitting `schema_version` silently drops your config — this cost a debugging cycle. Also, don't copy `BOOTSTRAP.md`; it would hijack every session.

**Rewrite (`learn`):**
> The first test is complete and it passed. Two common mistakes are important to know: if you omit `schema_version`, ZeroClaw silently ignores your config — this caused one extra debugging cycle. Also, do not copy `BOOTSTRAP.md`; it would take control of every session.
>
> Idioms in this response
> - "green" — passed (a test that succeeds)
> - "traps" — common mistakes that cause problems
> - "worth knowing about" — important to know
> - "cost a debugging cycle" — caused one extra debugging cycle
> - "hijack" — take control of something by force

## Quick-answer format

When the user asks only what a phrase means, answer directly — no rewrite:

> **"where things stand"** — the current status / the situation now (Spanish: en qué punto está todo). Literally "dónde están las cosas de pie", but nobody means standing anywhere.

## Reference

See `REFERENCE.md` for a table of common LLM-response idioms with literal replacements and Spanish hints. Load it when rewriting real chat or session responses.
