---
description: Verify every assertive sentence in a .tex file against disk and report verdicts; never edits.
---

Verify every assertive sentence in `$ARGUMENTS` against what is actually on disk, and report. This
command **never edits the `.tex` file** — not to fix a typo, not to close a bracket, not to correct a
number you are certain about. Silent correction is the failure mode this command exists to prevent.

Read `FORMAT.md` before starting; you cannot judge markup or notation without it.

## Scope

An *assertive sentence* is any sentence that states something checkable: a value, a comparison, a
cross-reference, a description of what a method does, an attribution. Skip motivation, transitions
and statements of intent — they assert nothing to verify.

Classify each one and run the matching check:

| Type | What to verify |
|---|---|
| Numerical value | The number appears in a result file under `D:\Projects\predictFTS` or in a source table. Name the file and the line or cell. |
| Figure reference | The `\label` is defined **and** the graphics file exists on disk at the path `\includegraphics` gives. Both halves, separately. |
| Table reference | The `\label` is defined **and** the table body is present in the file under review or in `best_predictor_aout24.tex`. |
| Equation reference | The `\label` is defined in `best_predictor_aout24.tex` or locally in the file under review. A label defined only in `main-2025-05-v2.tex` is **not** defined. |
| Method description | The described behaviour matches the code in `D:\Projects\adaptiveFTS` (what is implemented) or `D:\Projects\predictFTS` (what was run). Argument names in adaptiveFTS 0.3.0 differ from earlier releases — check the installed version, not your memory. |
| Citation | The key is present in `references_clean.bib`. |
| Markup | Conforms to `FORMAT.md`. Where `FORMAT.md` records a convention as unsettled, report the split rather than picking a side. |
| Notation | The symbol carries the meaning `FORMAT.md` assigns it. Flag any symbol whose meaning collides with another live use. |

## Verdicts

Exactly one per claim:

- **OK** — checked against a named source and it agrees.
- **MISMATCH** — checked against a named source and it disagrees. Give both values.
- **UNVERIFIABLE** — no source on disk settles it. This is a verdict, not a failure; it is the
  correct answer when the underlying result was never produced or the file is absent.
- **FORMAT-VIOLATION** — the markup contradicts `FORMAT.md`.
- **NOTATION-STALE** — the symbol is from a superseded version of the paper, or is used with a
  meaning `FORMAT.md` assigns to something else.

Never guess to avoid UNVERIFIABLE. A claim you could not check is more useful reported as unchecked
than reported as OK.

## Output

First, one table, one row per claim:

| File | Line | Claim | Type | Source checked | Verdict |
|---|---|---|---|---|---|

"Source checked" names a concrete path — the file you opened, not the category. For UNVERIFIABLE, name
where you looked and what was absent.

Then a **ranked correction list**, most consequential first. Rank by what a reader would be misled
about: a wrong number outranks a stale symbol, which outranks a markup slip. For each entry give the
location, what is wrong, what it should become, and which source establishes that. Where the fix is
not determined by a source, say so and put the decision to me rather than proposing a guess.

Close by stating how many claims you classified, how many landed in each verdict, and anything in the
file you deliberately did not classify.
