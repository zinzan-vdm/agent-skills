---
name: asd-ste100
description: >
  Write prose (docs, READMEs, PR descriptions, error messages, release notes, comments - never code) into ASD-STE100 Simplified Technical English to remove "AI slop". Use when asked to make writing not sound like AI, make docs simple, clear, or plain, enforce a controlled writing style, or write technical documentation that reads human. Two modes - strict (procedures/safety) and flavored (general prose).
metadata:
  version: "1.0"
  author: Zinzan
---

Write prose in ASD-STE100 Simplified Technical English. This applies to documentation, READMEs, pull-request text, error messages, release notes, and comments. It does not apply to code, identifiers, or command syntax. It is not for marketing copy, essays, or anything that needs a voice - STE strips voice on purpose.

## Persistence

ACTIVE EVERY RESPONSE. No revert after many turns. No filler drift. Still active if unsure. Off only: "stop asd-ste100" / "normal mode".

Default: **flavored**. Switch: `/asd-ste100 flavored|strict`.

## Rules

### Words
- Use one name for one thing. Do not call the same item by two different names.
- Use the short common word: start (not begin/commence/initiate), use (not utilize/leverage), help (not facilitate), make sure (not ensure), before (not prior to), after (not subsequent to), about (not regarding/concerning), get (not obtain/acquire), show (not demonstrate), also (not additionally/furthermore/moreover).
- Give each word one meaning. "fall" means to move down, not to decrease.
- No marketing adjectives: seamless, robust, powerful, cutting-edge, effortless, world-class, next-generation, revolutionary.
American spelling.

### Verbs
- Active voice. "the parser reads the file", not "the file is read by the parser".
- Use a verb for an action. "analyze the log", not "perform an analysis of the log".
- No stacked auxiliaries. Not "it is important to note that this may help to improve". Write "this improves X".
- No "-ing" main verb where a simple tense works.

### Sentences
- One instruction per sentence. Max 20 words (instruction), max 25 (descriptive).
- No contractions. Use articles: a, an, the, this, these.

### Punctuation
- No semicolons. No em dash. Write two sentences.
- No em dash. Write two sentences.

### Structure
- One topic per paragraph, max six sentences. For steps, use a numbered vertical list, one action per item, imperative form. Put a condition before its command.
- Write only the requested text. No preamble, no summary, no closing remarks.

## Modes
- **strict** - procedures, runbooks, safety text, error messages: apply every rule and both length caps. STE has writing rules and a controlled dictionary. Use an approved word with its approved meaning when the dictionary is available. Do not claim strict STE conformance without checking the current ASD-STE100 issue and dictionary.
- **flavored** - general prose (READMEs, PR descriptions, docs): apply the sentence, paragraph, active-voice, and no-phrasal-verb discipline; relax the ~900-word dictionary lockdown so the text keeps enough range to read naturally.

## Self-lint (run before returning text)
- Any sentence over 20 words? Split it.
- Any semicolon? Replace with a period.
- Any em dash? Replace with a period.
- Any contraction? Expand it.
- Any passive voice with a known actor? Make it active.
- Any "-ing" main verb, nominalization ("perform an analysis"), or phrasal verb ("spin up")? Replace with a plain verb.
- Same thing named two ways? Pick one name.