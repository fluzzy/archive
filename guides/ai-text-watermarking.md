# How AI Text Watermarking Works

> **Source**: [James Padolsey](https://declaude.org/watermarking/)
> **Author**: James Padolsey (NOPE)
> **Fetched**: 2026-09-07
> **Archived**: 2026-09-07

A visual explainer of the statistical watermarking schemes used by Gemini and Claude — how a secret key biases word choice, how a key-holder detects it, and why paraphrasing dilutes the mark while a true rewrite erases it. Published alongside `declaude`, a rewriting tool, so read the practical conclusions with that interest in mind.

---

## It Is Not a Character Trick

The first thing the guide rules out: this family of watermarks uses **no** zero-width spaces, no homoglyphs, no unicode oddities, and no em-dash tells. Nothing is inserted into the text. The mark lives in *which* words were chosen, so it survives copy-paste, retyping, and font changes — and it is invisible to inspection of the characters themselves.

## Writing as a Series of Small Choices

The mechanism rests on the fact that at most positions, several continuations are all fine. As the guide puts it, "a model writes by rolling weighted dice between several words that would each be fine." That slack is the carrier: bias the dice slightly and the text still reads naturally.

## A Secret Key Leans on Those Choices

The classic recipe (Kirchenbauer et al., 2023):

1. At each word fork, "secret-keyed maths splits the candidate words into **green** and **red**".
2. The split is **context-seeded** — "the key computes it from a short run of the words just before" the position. The same word is green after one prefix and red after another.
3. Green candidates get a probability boost. "The nudge is mild: a red word can still win — it's just a little less likely."

Two production variants:

| Scheme | How it differs |
| --- | --- |
| **Google SynthID** | Replaces the nudge with a secret *tournament*: candidates are drawn from the model's own odds, the key scores them, and the bracket is arranged so that "averaged over the key's draws, every word's odds stay exactly what the model intended" — the output distribution is preserved, not skewed. |
| **Aaronson's scheme** (OpenAI) | Skips biasing entirely and "derives the dice-rolls themselves from the key." |

## Detection: Counting, Not Reading

The detector replays the key's coloring over the suspect text and counts green words. It "doesn't read the text or judge its style."

- Unmarked text, or the wrong key: green wins about half the time — a coin flip.
- Marked text: green wins measurably more often.

The lean per word is small, so **confidence comes from length**: "small leans become persuasive only through length, which is why short texts are genuinely hard to call." The guide's worked example has a 1,500-word document flagging at only ~55% green — enough to be persuasive at that length, useless in a paragraph.

For specialists it gives a residual-evidence formula:

```
z ≈ f · √N · z₁
```

where `f` is the surviving fraction of marked windows, `N` the document length, and `z₁` the per-token strength.

Critically, **only the key-holder can run the test**. A teacher, an editor, or a public "AI detector" site cannot: a genuine check needs the provider's secret key.

## What Editing Does to the Mark

Because each word's color depends on the short run of words immediately before it, what matters is whether *runs of original wording* survive — not whether the meaning survives.

- **Light paraphrase dilutes rather than deletes.** In Kirchenbauer et al.'s experiments the detector recovers given enough text, "with even human paraphrase becoming detectable again after roughly 800 tokens (about 600 words)."
- **Rewriting from the meaning erases it.** Against open implementations, after a full rewrite "about 0.5% of windows survive, and detector accuracy falls from essentially certain to a coin flip." New phrasing produces coin-flip noise because the context windows no longer match.

This is the guide's central practical claim, and also the case for the tool it accompanies: a light pass that keeps most of the phrasing does not work.

## Limitations

1. **"Processed by", not "written by".** Anthropic's own documentation notes that human text merely proofread or translated by Claude picks up the mark.
2. **Absence proves less than presence.** Old models or heavy editing yield clean results on genuinely AI-written text.
3. **Short and low-choice text carries little mark.** Code, quotations, and lists of facts "offer the dice too little slack to hide anything in."
4. **Word-keyed schemes are more robust.** Schemes keyed on the word itself rather than its neighbours survive same-meaning rewrites far better — at the cost of being easier to reverse-engineer.
5. **Anthropic's production scheme is undisclosed**, so nothing here can be verified against Claude's actual mark.

## A Watermark Is Not an "AI Detector"

The guide is emphatic about the distinction: "Tools like GPTZero guess from style and are famously unreliable. A watermark is the opposite: a deliberate, key-gated statistical test. Don't let the two blur."

## Tools Mentioned

- **declaude** — the accompanying tool, whose "full-rewrite route" regenerates from meaning rather than preserving wording
- **Google SynthID detector portal** — early access
- Anthropic detection tooling — forthcoming as of writing

---

## References

- [How AI text watermarking works: a visual guide](https://declaude.org/watermarking/) — James Padolsey, NOPE
- Kirchenbauer et al. (2023), *A Watermark for Large Language Models* — the green/red list scheme
- [Google SynthID](https://deepmind.google/technologies/synthid/) — tournament sampling in production
