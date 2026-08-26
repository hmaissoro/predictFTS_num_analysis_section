---
description: Improve flow, clarity and STYLE.md consistency of a section or passage without touching mathematics or markup.
---

Improve the flow, clarity and consistency of `$ARGUMENTS` — a section name in this repository, or a
passage pasted directly. Read `STYLE.md` first; consistency with it is the target, and you cannot aim
at it from memory.

## What you may change

Sentence construction, paragraph order, connective tissue, register, the wording that points at a
float, the way an estimator or a competitor is named, and vocabulary that `STYLE.md` rules out. The
work is rewriting, not proofreading — reorder a paragraph if the argument arrives out of order.

## What you must not change

- **Mathematical content.** No symbol, subscript, index, quantifier, condition, rate, order of
  magnitude, or direction of an inequality. Rephrasing a sentence must not shift what it asserts.
- **Claims.** Add none. If a sentence gestures at something it never states, leave the gap — do not
  fill it. Strengthening a hedged claim is adding a claim.
- **Hedges that carry statistical meaning.** "provide evidence that", "is likely due to", "seems to",
  "up to logarithmic factors", "asymptotically", "in probability" are load-bearing. `STYLE.md` calibrates
  hedging; it never licenses removing a qualifier that marks the epistemic status of a result. Cut
  vagueness, never uncertainty.
- **Markup.** Not one backslash. No environment changes, no `\label` or `\ref` edits, no float
  repositioning, no equation reflowing, no citation command swaps. If markup is wrong, leave it and
  say so at the end — `/check-errors` is where that gets handled.

## How to mark the work

Every changed passage is wrapped in `\color{magenta} ... \color{black}`, never
`\textcolor{magenta}{...}`. Close the switch before `\end{equation}`, `\end{table}`, `\end{figure}`,
and before every `&` or `\\` inside a `tabular`. Emit no `%` comments.

Delete nothing. If a sentence should go, move it or flag it in magenta for my decision.

## Closing report

End every run with two lists, in the reply and not in the file:

1. **Left untouched because editing would have altered the mathematics.** Name each passage and say
   what the risk was — which symbol, condition or qualifier you would have had to move. Awkward prose
   that is mathematically load-bearing belongs here, not in the rewrite.
2. **Factual claims you could not verify.** Any number, cross-reference, sample size or method
   description you carried through without checking it against a file on disk. Carrying a claim
   forward is not endorsing it, and the distinction has to be visible. Do not verify them here — name
   them, and say that `/check-errors` is the instrument.

If either list is empty, say so explicitly. An absent list reads as an overlooked one.
