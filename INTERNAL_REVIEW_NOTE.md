# Note for internal reviewers: "ScoreSwap: How Many Model Versions Do Learned Exemplar-Memory Scores Survive?"

Draft of 26 Nov 2026. File: `internal_draft.pdf`. Please read this page first.

## What the paper asks

Some agent memories learn a usefulness score for each stored item. The score is learned under one model. When the
model is replaced by a later (or earlier) version, are the old scores still worth keeping? The paper measures how
much of their value is left as a function of **how many versions apart** the two models are (1 to 4 release steps),
in both directions, inside four open model lines (Qwen, Gemma, Granite, Phi; 18 small models that run on one free
GPU) and across lines. It also asks whether the loss compounds along a chain of upgrades, what kind of knowledge the
scores hold (shared by all models, by a line, or by one version), and what an operator should do at each distance
(keep, wipe, re-check, or shrink the scores by a factor learned on the other lines).

## What is real and what is simulated

- **Real:** one pilot on two closed models (Claude Haiku 4.5 and 5.5, one medical-diagnosis task set, 200 test
  tasks, 17,900 calls). It shows the scores carry a signal at home; it says nothing about version distance. Also real:
  an outside check on another group's released skill scores (no model calls), in the appendix.
- **Simulated:** every number, figure and table of the registered lineage study (Sections 5-7 and the lineage
  appendices). They come from 18 simulated stand-in models, labelled L1-v1 to L4-v4, with a decay that we planted.
  They show whether the analysis finds what was planted, and what this amount of data cannot detect; they say
  nothing about real models. **Read the dry run as a check of the design, not as findings**: it is one benchmark and
  two task orders, a third of the registered size, and it shows where the design is weak (a planted break at one
  release step was not found; the composition test detects only large departures). Every page carries the banner
  "INTERNAL DRAFT — RESULTS ARE SIMULATED (DRY RUN)", and every results caption starts with "SIMULATED".
- No simulated number is printed next to a real model's name. The real models appear only in Table 1, "Planned
  models (not yet run)".

## What will change when the real runs arrive

Only the numbers, figures and tables, and the tense of the sentences about what was run (one switch in
`internal_draft.tex`). The real runs need free GPU time: about 2 to 5 weeks of a free weekly quota for the first rung
(DDXPlus, one task order; an estimate until speeds are measured), run line by line: Qwen first, then Granite, Phi
and Gemma. The question, design, measures, statistics, benchmark, protocol and limitations are meant to
stay as they are, so that is where your comments matter most now. The real runs may overturn any simulated finding:
real lines may not decay smoothly, losses may not compose, and small models may fail the gate that memory must help
at home.

## The five questions we most want answered

1. Is the question worth a paper at a top venue? Is "how many versions do the scores survive" the right framing, or
   is it predictable ("nearer versions transfer better") unless the composition, three-level and lineage-prior
   analyses say something non-obvious?
2. Is the gap stated honestly (Sections 1-2 and Appendix H)? Do you know work we missed, especially on successive
   versions of one model line?
3. Is "version distance" defined well enough (release steps, months, the makers' architecture flag, behavioural
   distance), and are the confounds (capability gap, size jumps, training-data overlap) handled or stated well enough?
4. Would small open models (1-5 B) be accepted as the main evidence, with one closed-model pilot as the outside
   check? What one addition would most strengthen external validity?
5. Is anything in the draft unclear, over-claimed, or possible to mistake for a measured result? Does the dry run,
   read as a design check (Section 7, last paragraph), convince you that the real study is sized well enough?

## Practical

- Target: ICML 2027 (abstract 14 Jan, paper 22 Jan 2027, as posted on 26 Nov). Fallbacks: NeurIPS 2027 Evaluations
  and Datasets (dates not posted), ICLR 2028, or TMLR (rolling). ICML treats placeholder abstracts and invented
  references as grounds for desk rejection; every reference here was opened and logged.
- The submission version (`main.pdf`) has red placeholders instead of results. Please do not circulate either PDF
  outside the internal review.
- Known open items: the human authors still have to complete the AI Disclosure Statement, the anonymous code link, the
  registry link and the funding statement. In the dry run, re-checking with 64 checks falls below keeping and the
  level-corrected log score puts keep below wipe; both are limits of the simulator (stated as such in Section 7), and
  the real pilot shows the opposite on the level side.
