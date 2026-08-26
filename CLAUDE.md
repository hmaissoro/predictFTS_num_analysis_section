# CLAUDE.md

Instructions for working in this repository. Rules, not documentation.

This repo holds *Adaptive prediction for functional time series*. The numerical section is being
rewritten against a paper that is itself still being written, so most of what looks settled is not.

## Read before you act

Two files carry the standards. They are conditional reads — load them when the trigger fires, not
every session.

- **`STYLE.md`** — before writing or revising any prose, and before deciding whether a piece of
  material belongs in the main text or the supplement.
- **`FORMAT.md`** — before touching any `.tex` markup, and before using any symbol.

Do not reconstruct their contents from memory or from surrounding files. Open them.

## Authority

Three sources, in this order. A lower source never overrides a higher one.

1. **`best_predictor_aout24.tex`** — formulas, markup, notation. Sole authority. `notations.tex`,
   which it inputs, counts as its preamble.
2. **`D:\Projects\adaptiveFTS`** — what is implemented. If the paper describes an estimator the
   package does not provide, say so rather than writing around it.
3. **`D:\Projects\predictFTS`** — what was actually run. Numbers come from here or they are TBC.

`Numerical_analysis-2025-07-v1.tex` is the baseline being dismantled. It was written against a
superseded version of the paper; its notation, markup and cross-references are stale until verified
against the authority. `main-2025-05-v2.tex` is superseded orientation — never cite it as a source.

The JTSA paper (`paper_for_reference/JTSA/`) is a **calibration reference for prose and for the
deferral criterion only**. It never dictates markup, notation, the section plan, the number or
ordering of sections, the count of floats, or the length of anything. If you find yourself copying a
backslash out of a JTSA file, stop.

## Non-negotiable

**No LaTeX comments.** Never emit `%` as a comment in `.tex` output. Not to explain a choice, not to
park text, not to mark a spot. No exceptions. If something needs saying, say it to me in the reply.

**Mark every edit in magenta.** Every insertion and every modification is wrapped:

```
\color{magenta} ... \color{black}
```

Never `\textcolor{magenta}{...}` — it breaks across the constructs below. The switch is scoped, so
close it explicitly **before** `\end{equation}`, `\end{table}`, `\end{figure}`, and before every `&`
or `\\` inside a `tabular`. Reopen after the delimiter if the marking continues.

**Missing content is visible, never silent.** Where something is needed and not available:

```
\color{magenta}[TBC: what is missing and where it should come from]\color{black}
```

Name both halves — what is absent, and which file or run should supply it. A vague `[TBC]` is worse
than nothing because it looks handled.

**Never state an unverified quantity.** No number, no figure reference, no table reference, no sample
size goes into the document unless you have read it from a file on disk in this session. This covers
values recalled from earlier in a conversation, values inferred from a pattern, and values that
"must" follow from a formula. Anything unverified becomes a TBC placeholder. Cross-references are
included: check the `\label` exists before writing the `\ref`.

**Never delete content.** If material is wrong, superseded, or in the wrong place, either move it
where it belongs or flag it in magenta for my decision. Do not silently drop a paragraph, an
equation, a float, or a row of a table. Removal is my call, not yours.

## Working method

Work in logical increments and commit each one. Never commit to `main` — branch first. Conventional
Commits prefixes (`feat:`, `fix:`, `docs:`, `refactor:`, `chore:`). One coherent change per commit;
do not bundle a notation sweep with a prose rewrite.

Figure and table filenames are a contract with `\includegraphics` and `\input`. Renaming one means
renaming it in the `.tex` in the same commit.

When a source is silent or two sources conflict, do not pick for me. Do everything that does not
depend on the answer, put a TBC where it does, and raise the question in your reply.
