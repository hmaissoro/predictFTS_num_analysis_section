# STYLE.md — prose, deferral, figures

Learned from `paper_for_reference/JTSA/MPV_JSTA_revised.tex` and `MPV_JSTA_supp_final.tex` only. Calibration,
not a mould: it governs how a sentence is built and what earns the main text, never the section plan, the
count of floats, or the length of anything. No markup is recorded here.

## Part A — Prose

**Register and person.** First-person plural for every authorial choice — what we consider, propose, set,
conclude: *"We consider three types of FTS"*, *"we set gamma = 1/3"*. Passive or an impersonal subject for
what the data, the estimator or the machinery does, never for our own decisions: *"The results were obtained
using an R package"*, *"The rule 0/0 = 0 applies"*. The three authors are never named; the method is "our
procedure", "our local approach".

**Announcing a numerical result.** A fixed four-beat pattern, in order, usually one paragraph: name the float
and what it holds — *"Table 1 shows the bias and standard deviation of the estimates"*; state the reading,
flagged as expected or not — *"As expected the bias and the variance decrease"*; name the surprise if there is
one — *"which may be surprising given that the sample paths become smoother"*; attribute a cause — *"the
consequence is less precise estimates of the mean"*. The last beat is never skipped; a number is never left to
speak for itself.

**Sentences and paragraphs.** Twenty to thirty words, one subordinate clause at most; long chains break with a
semicolon or a new sentence, not with commas. Paragraphs open on the object under discussion and close on a
consequence or a restriction, never on a fresh fact — often beginning *"As a consequence"*, *"This explains
why"*, or *"To summarize"*.

**Pointing at a float, a table, a configuration.** The float is the grammatical subject, the verb is `shows` /
`presents` / `illustrates`: *"Figure 2 shows the boxplots of"*, *"In Figure 5 we present the boxplots of the
selected bandwidths"*. Parenthetical pointers use `see`. Sample sizes are always the ordered pair, introduced
once as a set — *"We consider (N, lambda) in {(150,40), (1000,40), (400,300)}"* — then referred to as *"four
setups (N, lambda)"* or *"in almost all setups"*. Never "scenario", never "case" for a configuration.

**Naming estimators and competitors.** Described in full at first mention with its authors and its mechanism,
then fixed to a short tag introduced by `referred to as`: *"procedure referred to as RP20"*. Our own estimator
gets a symbol, never a tag. The competitor's mechanism is restated once more, briefly, at the comparison.

**Unfavourable results.** Stated flatly in the main clause, then explained — never buried in a subordinate
clause, never softened by omission. Concession first, mitigation second: *"Although the ratio is close to 1,
our estimator shows slightly better performance"*. Limits on the theory are named as costs or as future work,
in one sentence, where the assumption is made: *"can be relaxed at the cost of more involved technical
arguments"*, *"we leave the study of this aspect for future work"*. Omissions are declared: *"For brevity, we
omit this extension."*, *"The details are omitted."*

**Hedging.** Three calibrated levels, used strictly. Evidence, not proof, for anything read off a plot: *"The
plots provide evidence that"*. A mechanism inferred but not verified: *"is likely due to"*, *"seems to fall at
the frontier"*, *"appear to capture this irregularity"*. Nothing hedged when the theory predicts it: *"As
expected"*, *"As anticipated from Theorem 2"*. A verdict is aggregated before it is given: *"Overall, the local
regularity estimators show good finite sample performance."* Concessions on the data are explicit: *"we decided
to neglect the missingness effect"*.

**Never used.** `outperform` — a comparison is a ratio with the criterion in parentheses, never a verb of
victory. `significantly` about any result, and no other intensifier: not `dramatically`, `substantially`,
`remarkably`, `vastly`, `strongly`. `clearly` or `obviously` as persuasion. Contractions, first-person
singular, rhetorical questions, exclamation. `we believe`, `we feel`, `interestingly`, `it is important to
note`. A paragraph opening with `However`. A float referred to without a verb saying what it shows.

## Part B — The deferral criterion

**The rule.** Material stays in the main text if a reader who accepts our claims needs it to *understand what
was done and judge whether it worked*; it is deferred if needed only to *reproduce or re-derive* the work:

- Estimator definition, the risk it minimises, the rate it attains — **main**; the proof of that rate and every
  technical lemma feeding it — **deferred**.
- One representative configuration, shown in full — **main**; the remaining configurations, when they say the
  same thing — **deferred**, with the sameness stated rather than left to the reader.
- A tuning parameter's choice and outcome — **main**; the experiment producing it — **deferred**.
- A comparison against a competitor — **main**, in one representative setting only.
- An assumption's role and cost — **main**; verifying it on the data — **deferred**.

Close calls: if removing it leaves a claim unsupported, it stays; if it only leaves a reader unable to rebuild
the number, it goes.

**Signalling a deferral.** Short, terminal, no apology, at the end of the sentence or as its own: *"Details are
provided in the Supporting Information."*, *"are presented in Supporting Information"*, *"see Supporting
Information for the variance plot"*. The main text never summarises what it defers beyond naming the object.

**Pointing back.** The supplement names the main text once at the top and thereafter treats it as shared
context: *"to which we refer to as `main manuscript'"*, *"introduced in the main manuscript"*. Each deferred
section opens by restating its own job and its origin — *"In this section we give more details on the
simulation setting of Section 5"* — and, where it repeats a main-text finding, says so and does not re-argue
it: *"Recall that the FTS Model 1 setup is similar to FTS Model 2, the results of which are already presented
in the main paper"*.

## Part C — Figure design habits

**Panel arrangement.** A single row of two or three, or a single column, never a dense grid. Each panel is one
point of the domain or one model, and the panels differ in exactly one variable. Panel identity sits in the
caption by position — **Left:**, **Middle:**, **Right:** — not in letters inside the image.

**What a comparison figure shows.** Never two methods' outputs side by side in separate panels. Either both are
drawn on the same axes with distinct line styles, or the comparison is reduced to a single derived quantity — a
ratio, or the selected bandwidths themselves — and shown as boxplots whose reference value (1, or the true
value) is drawn as a dashed line.

**Encoding sample-size configurations.** All four `(N, lambda)` pairs go on one set of axes, distinguished by
line style and keyed in the caption by an inline sample of the line itself with the pair spelled out beside it.
When the quantity is a distribution rather than a curve, the configurations become adjacent boxplot groups
along the horizontal axis and the domain point moves to the panel. Never distinguished by colour alone.

**Captions.** Two to four sentences: what is plotted, at which points of the domain, over how many replications,
under which model — *"over 400 independent functional series generated according to FTS Model 2"* — then a
decoding of every visual channel used. No interpretation; that belongs to the paragraph pointing at the figure.

## Departures from JTSA required by this paper

- JTSA offers no pattern for a tuning parameter that is not a bandwidth; the regularisation parameter needs one.
- Partial-observation scenarios add a third axis beside N and lambda, beyond JTSA's two-axis encodings.
- JTSA estimates a function, this paper predicts a curve, so comparisons here are per-replication error ratios
  rather than bias and standard deviation tables.
