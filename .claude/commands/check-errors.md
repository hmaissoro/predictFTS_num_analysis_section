---
description: Verify every assertive sentence in a .tex file against disk and report verdicts; never edits.
---

Verify every assertive sentence in `$ARGUMENTS` against what is actually on disk, and report. This
command **never edits the `.tex` file** — not to fix a typo, not to close a bracket, not to correct a
number you are certain about. Silent correction is the failure mode this command exists to prevent.

Read `FORMAT.md` before starting; you cannot judge markup or notation without it. Read `RUNLOG.md`
and `ASSETS.md` too: they record what was actually run and which figure is which, and the numerical
and figure checks below resolve against them.

`RUNLOG.md` and `ASSETS.md` are generated maps and go stale the moment predictFTS is re-run. Where
one of them disagrees with disk, disk wins — and say so in your report, because a stale map is
itself a finding.

## Scope

An *assertive sentence* is any sentence that states something checkable: a value, a comparison, a
cross-reference, a description of what a method does, an attribution. Skip motivation, transitions
and statements of intent — they assert nothing to verify.

Classify each one and run the matching check:

| Type | What to verify |
|---|---|
| Numerical value | Resolve against `RUNLOG.md` first — it names the result file each quantity comes from, and its Table of not-established items says which quantities no file settles. Where `RUNLOG.md` does not cover the claim, go to the result files under `D:\Projects\predictFTS` directly. Name the file and the line or cell either way. `ASSETS.md` Table C already carries a verdict for every number currently in the two `.tex` files; treat it as a prior to confirm, not as the answer. |
| Figure reference | Three halves, each reported separately. (1) The `\label` is defined. (2) The graphics file exists on disk at the path `\includegraphics` gives. (3) `ASSETS.md` Table A carries a row for that path, and its verdict is MATCH or RENAMED. |
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

Three rulings that the two maps make possible, and that were previously guesswork:

- A value `RUNLOG.md` records as coming from the **superseded estimated-`H_t` run** is a
  **MISMATCH**, not UNVERIFIABLE. The run that produced it has been replaced; the number is wrong,
  not merely unchecked.
- A figure whose `ASSETS.md` Table A verdict is **SUPERSEDED** is a **MISMATCH**. The file is
  present and will typeset, so the reference itself is sound — but the figure no longer shows what
  it showed. Check the claim against Table A's "what it now shows" line and report the prose that
  the change invalidates.
- A figure path with **no row in Table A** is **UNVERIFIABLE**, and say plainly that the map needs
  regenerating: either the `.tex` gained a reference after `ASSETS.md` was built, or the path is
  wrong.

A `[TBC: …]` note asserting that a result, directory or file does **not** exist is a checkable
claim like any other. Several such notes in the two `.tex` files were true when written and are
false now. Check them; do not skip them as commentary.

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
