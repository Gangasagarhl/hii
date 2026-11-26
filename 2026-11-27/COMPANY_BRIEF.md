# When your model is retired, what happens to what your agent has learned?

**A one-page brief for teams that run AI agents with stored examples or memory**

## The problem

Your product runs on a language model that its maker will switch off on a date you do not choose. One maker promises "at least 60 days' notice"; one cloud platform upgrades some deployments to the replacement model automatically at retirement. In a study of open-source applications, an estimated 82% of migrations away from retired models were committed after the shutdown date.

Over time your product has learned which stored examples help: verified example queries, resolved cases, upvoted memories, feedback on answers. Those signals were earned under the old model. Published studies show that examples that help one model can hurt the next, and that an update can turn right answers wrong even when the average improves: in one study of API updates, 58.8% of prompt and model combinations lost accuracy.

Migration tools today rewrite prompts or re-run a test set. In the documentation we read, none says what to do with stored memory or its learned scores. On swap day teams choose between keeping everything, wiping everything, or re-testing everything.

## What we built

**ScoreSwap** is an open method and tool that answers three questions before the new model goes live:

1. **How much of the learned value survives** the move to the new model.
2. **What keeping costs**: how often keeping the old scores makes a right answer wrong (regressions) against how often it fixes one, per 1,000 tasks.
3. **What to do**: keep, check first, or wipe, decided by a rule written down in advance, from a small random sample of checks on the new model, with a stated error rate, and with the cost in calls and tokens.

The tool works offline on your own logs: the examples stay fixed and only the decision is tested. Nothing has to be retrained.

## Where the work stands

- The method, statistics and scoring tool are built and tested.
- A pilot on two commercial models of one maker is done.
- A study on 18 open models from four model families, across one to four releases apart, in medical, customer-support and database-question tasks, is designed and is being run. Its results are not available yet.

## What we propose

A short pilot on your next model upgrade:

- You share (under your terms) a sample of stored examples with their feedback signals, and a set of test cases.
- We run the swap-day check for the old and the new model and return a short report: value kept, regressions and fixes per 1,000, the recommended action, and what the check cost.
- You keep the report and the tool. With your agreement, an anonymised case study goes into the research paper.

## Contact

[name] · [email] · [link to the paper and the code]

---
Sources for the figures above: Anthropic model deprecations page; Microsoft Azure OpenAI model retirement pages; Kim, "When the Model Retires" (arXiv 2609.31288, 2026); Ma, Yang and Kästner, "(Why) Is My Prompt Getting Worse?" (CAIN 2024).
