# Measure How Fast Inherited Memory Scores Fade

**As of 26 November 2026, nobody has measured how quickly the learned usefulness scores in an agent's memory lose their worth when the language model is replaced by a version one, two, three or four releases away, along complete open model lines and across them. Nobody has scored the swap-day choices (keep the scores, wipe them, or recalibrate them from a few fresh checks) against a check budget on the same frozen items either.** That is the open gap, and it is narrow. Each close rival owns one piece. UpgradeBench found that fine-tuned add-ons carry across Qwen releases on "a distance budget" ([UpgradeBench](https://arxiv.org/html/2608.20918)). MemRL moved a scored memory between makers but never separated the scores from the stored text ([MemRL](https://arxiv.org/html/2601.03192v2)). Beyond Gain and Yang et al. compared per-item usefulness across models against rerun noise, with no version axis ([Beyond Gain](https://arxiv.org/html/2610.32322); [Yang et al.](https://arxiv.org/html/2611.03930)). HEIRS re-tests archived skills across Qwen3.5 upgrades, but its full text is still unread ([HEIRS](https://openreview.net/forum?id=Ds8Nz1GuuQ)). All four new ideas can be claimed, but only as new uses of old tools, and each tool must be credited. The compounding test is a 1960s test–retest model. The split of "what survives" is standard variance analysis. The zero-check swap-day rule is a discounted-prior rule whose discount is learned from other lines. Behaviour-based distance measures already exist, so only using them to forecast score survival is new. The study can run on free hardware: about 18 models of 1.5–4 billion parameters in the Qwen, Gemma, Granite and Phi lines fit Kaggle's two free 16 GB T4 GPUs in 16-bit. Gemma is the weak link (Gemma 2 and 3 are forced to 32-bit, and Gemma 4 will not run in vLLM on a T4). Our rough arithmetic puts the core runs at 30–60 quota hours against an unofficial, floating allowance of about 30 hours a week. Reviewers will say "small models only", "distance is ill-defined" and "the result is predictable". Each objection has an answer, and each answer must be built into the design before the first run. ICML 2027's evaluation-and-benchmark contribution type fits best: abstract 14 January, paper 22 January 2027 ([ICML Dates](https://icml.cc/Conferences/2027/Dates)). NeurIPS 2027 Evaluations & Datasets (dates not yet posted) and TMLR are the fallbacks. Nothing here guarantees acceptance.

## Nobody has drawn the fade curve for learned memory scores

### The picture: a notebook of scored tips changes hands

Picture a garage. Mechanic A keeps a notebook for a year. Each page holds one tip, such as "if the engine knocks, check the plug gap first". Next to each tip is a score: how often that tip fixed the car when A tried it. Then A leaves and B takes the bench. Should B trust A's scores? That depends on who B is. B might come from the same training school one class later, or four classes later, or from a different brand's school altogether. The paper asks two questions. First, how fast do A's scores lose their worth as B gets further from A? Second, what should the owner do on B's first morning: keep the scores, tear them out, or test a few tips with B and adjust the rest?

| In the garage | In the agent |
|---|---|
| The tips in the notebook | Memory items (stored lessons or worked examples), kept identical for every model |
| The score next to each tip | A learned usefulness score per item, such as the running average of success when the item was shown |
| The mechanic | The language model that reads the memory |
| Same school, one class later | The next version in the same line, such as Qwen2 → Qwen2.5 |
| A different brand's school | Another line, such as Qwen → Granite |
| How many classes apart | Version distance: release steps, months, or measured behaviour |
| B's first morning | Swap day |

One choice keeps the study clean: the tips stay the same for every mechanic, and only the scores move. That separates this paper from work that asks whether the notes themselves still make sense to a new model.

### What earlier work did, and how close it comes

Earlier work has every ingredient, but no single paper combines them. Five ingredients matter here: a learned score per item; memory or examples as the thing being scored; successive versions of one line; distance as the thing being varied; and a rerun of the same model to show how much difference is just noise. The table lists the closest works and what each leaves open.

| Work | What it measured | How close it comes | What it leaves open |
|---|---|---|---|
| UpgradeBench, Aug 2026 ([HTML](https://arxiv.org/html/2608.20918)) | Fine-tuned add-ons carried along Qwen1.5 → 2 → 2.5 → 3. Retention falls from 0.88–0.99 down to the no-add-on floor as continued training grows from 46B to 2.9T tokens | Closest on distance: "Portability is not a family property but a distance budget" | Model weights, not memory. Distance in training tokens. Multi-step Qwen ports not measured. Fitting the decay curve is listed as future work |
| MemRL, App. C.1 ([HTML](https://arxiv.org/html/2601.03192v2)) | A frozen, scored memory from Gemini-3-pro lifted Qwen3-235B on HLE from 0.150 to 0.531 | Closest on moving learned scores | Cross-maker only. No control that keeps the text but drops the scores. No noise reference |
| Beyond Gain, Oct 2026 ([HTML](https://arxiv.org/html/2610.32322)) | Skill verdicts flip for 14–23% of skills between models, against 5–9% when one model is simply rerun | Per item, with a noise reference; includes Qwen3.6-27B and Qwen3.8-27B | Skills, not learned memory scores. No distance axis |
| Yang et al., Nov 2026 ([HTML](https://arxiv.org/html/2611.03930)) | Memory-task usefulness labels agree at κ = 0.58 across two reader models, against 0.65–0.78 on a rerun | Memory, with a noise reference | One cross-maker swap. Measured labels, not learned scores |
| HEIRS, ICLR 2027 submission ([OpenReview](https://openreview.net/forum?id=Ds8Nz1GuuQ)) | Re-testing archived skill versions across "Qwen3.5 migrations" and one DeepSeek → GLM swap raises ALFWorld success by 4.5 points over keeping the latest version | Closest swap-day rule | Full text unread. Unknown whether it reports results step by step |
| Yu, Zuccon & Leelanupab, Oct 2026 ([arXiv](https://arxiv.org/abs/2610.09820)) | Relevance judging along Qwen 1.5 → 2 → 2.5 → 3 → 3.5 (7–9B) and Llama 2 → 3 → 3.1 | Same Qwen line, same five versions | Judgements, not item usefulness. No decay or compounding analysis |
| Judge-replacement paper, desk rejected ([OpenReview](https://openreview.net/forum?id=fmDOTwVZes)) | Keep vs rescore after "14 adjacent and 4 cumulative" judge replacements | The only adjacent-vs-cumulative comparison found | Judge scores, not memory |
| "Same Papers, Different Verdicts", Nov 2026 ([arXiv](https://arxiv.org/abs/2611.20608)) | Across two Claude Opus versions, stored review scores keep their order (r = 0.73) while the accept share moves 44 points | Shows that order and level can split apart | Commercial judge scores, not memory |
| Goyal & Ray, Sep 2026 ([arXiv](https://arxiv.org/abs/2609.05339)) | Whether memory formats survive one swap between two open models under 10B | Same question in its title | Stores no learned per-item scores. One cross-maker swap |
| Chang & Jia, ACL 2023 ([PDF](https://aclanthology.org/2023.acl-long.452.pdf)) | Per-example usefulness correlates from −0.03 to 0.76 between models; 0.76 between two OPT sizes | Per item, across models | No versions, no noise reference |
| MUSCLE, 2024 ([HTML](https://arxiv.org/html/2407.09435v2)) | Answers that were right and turn wrong after Llama 1 → 2 and Phi 1 → 1.5 → 2 | Per item, across versions | The model's own answers, not item usefulness. Skip-version pairs listed but not analysed |

The last week added no rival. From 20 to 26 November, new papers learned item usefulness but never swapped the model. Examples are Leto's "utility loop" ([arXiv](https://arxiv.org/abs/2611.33005)), BRAG's utilities that "drift over time" ([arXiv](https://arxiv.org/abs/2611.31656)) and AdMem's single usefulness number per entry ([arXiv](https://arxiv.org/abs/2611.20605)). Engram checks "whether that evidence applies to the model currently being tuned", but its models are the ad models being tuned, not the model that reads the memory ([arXiv](https://arxiv.org/abs/2611.28356)). MoHA found that tuned harnesses "generalize only partially to other models" ([arXiv](https://arxiv.org/abs/2611.27646)). That supports the paper's premise, but MoHA has no scores and no versions.

Two signals matter for strategy. **At least four ICLR 2027 papers proposing new learned-utility memories now show "Desk Rejected"**, for example [MemEff](https://openreview.net/forum?id=m0bPKwwIMl) and [MEM-EVOLVER](https://openreview.net/forum?id=Ynzj6P6GxF). The reasons are not public. Even so, the pattern says a new scoring method is a weak entry and a careful evaluation is the stronger one. Second, the ICLR author–reviewer discussion closed on 18 November ([comment](https://openreview.net/forum?id=6dLt4SuFjf&noteId=Lj0gZTBvAA)). No rival can add rebuttal experiments before the 16 December decisions.

### The gap in one sentence

**No published or under-review work measures how much of the decision value of learned per-item memory usefulness scores survives a model replacement, as a function of measured version distance along complete open model lines and across lines. None scores keep, wipe and recalibrate-from-k-checks against a check budget on the same frozen items.** Existing evidence inside one line covers one or two version pairs per paper. Examples are HEIRS's Qwen3.5 migrations, the two Qwen updates in the withdrawn Policy-Response Drift paper ([OpenReview](https://openreview.net/forum?id=6YeGSweiCZ)), and the judge paper's adjacent-versus-cumulative comparison. No paper sweeps distance along three or more full lines.

Two developments could shrink the gap. If HEIRS's PDF reports results step by step inside its Qwen3.5 migrations, the gap narrows to "learned per-item scores and several lines". ICLR decisions on 16 December, plus camera-ready additions, could also move HEIRS or similar papers closer. The searches behind this verdict had limits too. The arXiv interface was rate-limited, and ICLR 2027 reviews could not be read by machine.

**What to do.** Read the HEIRS PDF in a normal browser this week. Repeat the novelty search around 5 January, after the ICLR decisions. Cite everything made public before about 22 November. ICML does not require authors to discuss work made public less than two months before its 22 January deadline ([Peer Review Ethics](https://icml.cc/Conferences/2027/PeerReviewEthics)). By that rule, Leto (24 November), BRAG (23 November) and the 23 November paper on composing distilled memory policies ([arXiv](https://arxiv.org/abs/2611.30230)) are concurrent work. "Same Papers", AdMem, Engram and MoHA fall just before the line, so cite them as prior work.

## Four new ideas are new uses of old tools, so credit the tools

None of the four ideas is a new method. Each asks a new question with a tool built in another field. Reviewers accept that, but only when the paper names who built the tool and claims just the measurement and its finding.

### Idea 1: Does the loss compound from step to step?

**The picture.** The notebook passes from A to B to C. Suppose each handover keeps 80% of the scores' worth. Does C end up with 64%, the way interest compounds? Or does C keep more, because all three learned at the same school and share a core that never fades?

**What exists.** Nobody has tested this on language-model release chains. UpgradeBench measures single steps only and lists fitting "the retention-vs-distance decay curve" as future work ([UpgradeBench](https://arxiv.org/html/2608.20918)). Qiao, Ding and Fan track "cumulative drift" and "step drift" across 32 models from six families, but never compare the two ([HTML](https://arxiv.org/html/2608.11027v1)). Yu et al. walk the same five Qwen versions without any compounding test ([arXiv](https://arxiv.org/abs/2610.09820)).

The rule itself is old. Psychology's test–retest "simplex" model assumes exactly this multiplying chain. Its three-wave estimates are known to be fragile: "minor violations of this assumption can lead to a large bias" ([Coenders et al. 1999](https://merit.url.edu/en/publications/stability-of-three-wave-simplex-estimates-of-reliability/)). In machine learning, face-recognition models trained to stay compatible were "transitively compatible" across three versions, with the two-step pair scoring below the one-step pair (53.98 against 56.34) ([BCT, CVPR 2020](https://ar5iv.labs.arxiv.org/html/2003.11942)). PhyloLM defines the distance between two models as minus the log of their similarity ([PhyloLM](https://arxiv.org/html/2404.04671v3)). Under that definition, multiplying similarities is the same as adding distances, though PhyloLM never tests whether that holds.

**How to test it without fooling yourself.** First measure how well the scores agree between versions 1 and 2, between 2 and 3, and directly between 1 and 3. Then correct each figure for noise, by dividing it by how well each version agrees with itself when simply rerun. Finally, check whether the 1-to-3 figure equals the two short steps multiplied together. The noise correction must come from real reruns, never from the multiplying rule itself. Otherwise the test assumes what it sets out to prove. Having four or five versions per line also makes the test possible. With five Qwen, five Granite, four Phi and four Gemma versions, the design holds 28 three-version chains (10 + 10 + 4 + 4, our count).

**Why it matters.** Suppose each upgrade keeps a share r of the value. Then n upgrades without refreshing keep r multiplied by itself n times. The number of upgrades until half the value is gone is 0.69 ÷ (−ln r). At r = 0.8, that is about three upgrades (our arithmetic). If the long step keeps more than the product, the line has a floor that never fades, which ties directly to Idea 2.

**How to claim and credit it.** Write: "To our knowledge, no prior work tests whether the loss of transferred value compounds along a chain of language-model releases. The rule we test is the lag-1 Markov ('simplex') structure of test–retest panels (Heise 1969; Wiley & Wiley 1970). It is equivalent to adding −log similarities, as in PhyloLM. Transitivity across versions was shown for compatibility-trained embeddings (Shen et al. 2020)." The Heise and Wiley & Wiley texts were not checked against the originals, so read them before citing. Do not call the rule a "law".

Expect two objections. The first: with only four or five versions, the test has a low chance of detecting a real effect. The second: "version 2 sits between versions 1 and 3" is undefined when sizes and tokenizers change from one release to the next. Answer with noise corrections from reruns, resampling intervals on the gap between the long step and the product, a stated check of how big an effect the test could detect, and size and accuracy change held fixed in the analysis.

### Idea 2: What kind of knowledge in a score survives?

**The picture.** Some tips work for any mechanic ("check the battery first"). Some work for anyone from the Toyota school. Some only worked for A's own habits. If most of a score's worth is the first kind, any new mechanic can trust it. If most is the third kind, the scores lose their worth when A leaves.

**What exists.** Two-part splits are already published:

- **Beyond Gain:** the skill together with its task explains 62% of the score variation, the clash between skill and model 15%, and overall model strength only 0.4% ([Beyond Gain](https://arxiv.org/html/2610.32322)).
- **Kriegmair & Wulff:** across 74.9 million word ratings from 10 open models, 16.9% of the variation is model-specific "individuality" beyond noise ([Kriegmair & Wulff](https://arxiv.org/abs/2604.16755)).

Agreement is also known to be higher inside a family:

- 0.76 between two OPT sizes, against near zero across families ([Chang & Jia](https://aclanthology.org/2023.acl-long.452.pdf)).
- Errors line up more often for models with "shared architectures and providers" ([Kim et al., ICML 2025](https://arxiv.org/abs/2506.07962)).
- Item-by-family effects reorder benchmark rankings: 30.9–47.1% of near-tied cross-family pairs swap places in four of five benchmarks ([Zheng & Yang](https://arxiv.org/abs/2609.00482)).

**No paper splits per-item usefulness into three parts (universal, line-level and version-level) with rerun noise removed.**

**The tool's real name.** This is generalizability theory, also called a crossed random-effects model. Items are crossed with versions, versions are nested inside lines, and reruns supply the noise. Plant breeders use the same design for how varieties behave in different environments. In their words, "only those environments with similar characteristics lead to a strong positive Type B genetic correlation" ([VSNi](https://vsni.co.uk/blogs/gxe-interaction)). Claim the application and the numbers, not the method.

**How to claim and credit it.** Write: "Per-item usefulness is known to be largely model-specific (Chang & Jia 2023; Beyond Gain 2026), and agreement is known to be higher within a family or provider (Kim et al. 2025; Zheng & Yang 2026). We know of no study that, with models from several lines reading the same stored items and reruns removing noise, separately estimates the universal, line and version shares. We estimate them with standard variance components." If the data show it, the new result is how large the line share is compared with the version share. A line share above zero is exactly what would let the long step in Idea 1 keep more than the product.

**The risks.** With only four lines, the line share is like an average taken from four numbers, so its range of uncertainty will be wide. Each maker contributes exactly one line, so "line" cannot be told apart from "maker". Learned scores are also noisier for items shown less often. Answer with a standard mixed-model fit that reports honest intervals, a check that the shares stay stable when each line is left out in turn, per-item noise estimated from how often each item was shown, and a plain statement that line equals maker in this design. The full texts of Zheng & Yang, Kriegmair & Wulff and a related agent-reliability paper ([Hardy et al.](https://arxiv.org/abs/2610.00651)) were not opened. Before submitting, confirm that none of them already has a family level.

### Idea 3: A swap-day rule that needs no fresh checks

**The picture.** On B's first morning there is no time to test tips. The owner remembers three other garages, though. There, when a notebook passed one class down, about 70% of the scores' worth carried over; across two classes, about 50%. So he tells B: "Trust these scores, but shrink them to 70%." No tests are needed, only the history of the other garages.

**What exists.** The estimator is well known under other names:

- **Power prior.** Borrows old data at a discount ([Ibrahim & Chen](https://doi.org/10.1214/ss/1009212673)). Gravestock and Held choose the discount from the data, but they use current data, which a zero-check rule by definition does not have ([arXiv](https://arxiv.org/abs/1708.08239)).
- **Reznik 2026.** Shows that the best discount shrinks as old and new data grow apart: "compatible data are pooled in full, and any difference caps the borrowed information" ([arXiv](https://arxiv.org/abs/2608.12159)).
- **Meta-Thompson Sampling.** "Meta-learns the prior" from earlier decision problems ([ICML 2021](https://arxiv.org/abs/2102.06129)). This is the closest relative of learning the discount from other lines.

Three language-model works come close without touching memory scores:

- **RouteProfile.** Routes brand-new models that have no usage data, and finds "model family metadata is more reliable than benchmark domain information" ([arXiv](https://arxiv.org/abs/2605.00180)).
- **LRMF.** Uses lineage to predict new models' scores "even with minimal data" ([arXiv](https://arxiv.org/abs/2504.19811)).
- **memscore.** A GitHub tool that imports memory scores with fixed weights. It runs 40 probe tasks first and ships only a synthetic test. In that test, plain import (0.831 success) beat its own recalibration (0.795) ([memscore](https://github.com/habib-analyst/memscore)).

**No work sets a zero-check shrink factor for learned per-item scores from other model lines at the same version distance.**

**The honest limit.** Shrinking every score by the same factor cannot change their order. So this rule cannot make old items rank better among themselves. It matters in three situations:

- when old items compete with new items that have no score yet;
- when scores are mixed with text similarity;
- when the number itself is used, as in "this tip works 80% of the time".

Medicine learned the same lesson when moving prediction models to new hospitals: recalibration "improved the calibration" but "discrimination could not be improved" ([Janssen et al.](https://pubmed.ncbi.nlm.nih.gov/18083464/)). "Same Papers" shows why the level still matters: order held at r = 0.73 while the level moved 44 points ([arXiv](https://arxiv.org/abs/2611.20608)).

**The strongest warnings.**

- **Mondal et al.:** reusing old evidence with zero new checks rests on an assumption that "must be assumed, not learned". In their test data, a zero-label approval would have covered 81% of cases while the assumption behind it held for only 35% ([Mondal et al.](https://arxiv.org/html/2609.32267)).
- **Beyond Gain:** "a verdict has to be re-read for the model it will govern" ([Beyond Gain](https://arxiv.org/html/2610.32322)).
- **Afrin & Shihab:** in recalibrating LLM judges for a new domain (not a new model), fitting 16 fresh labels caused harm 9% of the time, against 34% for carrying the old calibration across ([Afrin & Shihab](https://arxiv.org/abs/2609.27954)).

**How to claim and credit it.** Write: "We know of no prior method that carries learned per-item usefulness scores across a model swap without new evaluations and sets its shrinkage from other model lines at the same version distance, validated by leaving one line out. The estimator is standard discounted-prior or meta-learned-prior shrinkage (Ibrahim & Chen; Gravestock & Held 2017; Reznik 2026; Kveton et al. 2021)."

Then show it beats four simple rivals: one fixed factor for all swaps, a reset to the default, each line's own best factor (a cheating upper bound), and no transfer at all. Report the loss over the first tasks after the swap, not just the value on day one, because the rule's edge could vanish after a handful of fresh results. Add a cross-line swap where the rule should not help, as a deliberate negative test. And state plainly that release steps differ between lines: Qwen3 added a thinking mode, while Phi-2 → Phi-3 changed size and training data.

### Idea 4: Distance measured from behaviour

**The picture.** Instead of asking which class B graduated in, give A and B the same ten broken cars without the notebook. Then see whether they make the same mistakes. Two mechanics who fail on the same cars probably value the same tips.

**What exists.** The measures themselves are published:

- **CAPA.** Counts shared mistakes beyond chance. Its authors found it tracked the gain from weak-to-strong training at r = −0.85, but only about −0.35 once the accuracy gap between the models was taken out. It needs token probabilities and multiple-choice tasks ([Goel et al.](https://arxiv.org/abs/2502.04313)).
- **PhyloLM.** Builds family trees of models from their outputs ([PhyloLM](https://arxiv.org/html/2404.04671v3)).
- **Jalouzot et al.** Across 43 models from 10 families, release date "is the strongest predictor" of how similar two models are ([Jalouzot et al.](https://arxiv.org/html/2406.12620v3)).
- **Sheng & Lu.** Signals such as drift between versions sometimes predict which answers break in an update, but "no signal is universally best" ([Sheng & Lu](https://arxiv.org/abs/2608.13607)).
- **Baek et al.** A published theory already links agreement between models to how often a ranking stays correct. At its simplest point it gives ½ + arcsin(ρ)/π ([Baek et al., AISTATS 2025](https://proceedings.mlr.press/v258/baek25a.html)).

**No work uses a memory-free behavioural distance, measured before the swap, to forecast how much of a memory's learned scores will survive, tested on lines left out of the fit.**

**Why local open models help.** CAPA needs token probabilities, and many paid APIs do not return them. A model served on your own GPU does, so the measure costs nothing extra for every pair.

**The trap reviewers have already set.** A PhyloLM reviewer asked what a measured tree adds when users already know "which generation they are from" ([review](https://openreview.net/forum?id=rTQNGQxm4K&noteId=ysd2GWVrQF)). Stronger models also share more errors simply because they get more answers right, "even with distinct architectures and providers" ([Kim et al.](https://arxiv.org/abs/2506.07962)). So behavioural distance earns its place only if it predicts survival better than three simpler measures: the release step, the months between releases, and a same-line yes/no. It must do so after the accuracy gap and model size are taken out.

**A natural experiment inside the lines.** Some steps keep the model's shape exactly and change only the data and training: Qwen2 → Qwen2.5 at 1.5B, Phi-3-mini → Phi-3.5-mini, and Granite 4.0-micro → 4.1-3B. Other steps change the architecture, the tokenizer and more at once, such as Qwen3 → Qwen3.5 ([Qwen3.5 card](https://huggingface.co/Qwen/Qwen3.5-2B)) and Phi-3.5 → Phi-4-mini ([Phi-4-mini card](https://huggingface.co/microsoft/Phi-4-mini-instruct)). UpgradeBench found that copying add-ons "fails between independent pretraining runs". Its retention on the Spider task swung from 0.932 to 0.414 to 0.578 over three successive Qwen steps ([UpgradeBench](https://arxiv.org/html/2608.20918)). So the loss could sit at a few big steps instead of spreading evenly. That is the kind of non-obvious, decision-useful finding the paper needs.

### How the four ideas join into one story

The four ideas form one chain:

1. Idea 4 measures, before the swap, how far apart two models are.
2. Idea 1 says how that distance adds up over several steps.
3. Idea 2 says how much of each score belongs to the whole line, and therefore how far a factor learned on other lines can travel.
4. Idea 3 turns all of it into a rule for swap day.

The research notes derive (this is not from a published source) that, under simple assumptions, the right day-one shrink factor equals the noise-corrected agreement between old and new scores, times the ratio of how widely each set of scores spreads. So the same quantity runs through all four ideas. The answer the owner actually needs comes at the end: **how many fresh checks a swap of a given distance needs before recalibrating beats keeping**.

| Idea | Verdict on 26 Nov 2026 | What you can claim | Who you must credit | Biggest risk |
|---|---|---|---|---|
| 1. Does the loss compound? | Not tested on language-model releases; the rule is an old test–retest model | First test on release chains, for item usefulness | Heise; Wiley & Wiley; Coenders et al.; PhyloLM; BCT; UpgradeBench | Weak test with few versions; circular if noise is taken from the rule; steps are unequal |
| 2. What survives? | Two-part splits exist; a three-part split of usefulness was not found | First universal, line and version shares of per-item usefulness | Generalizability theory; plant breeding; Beyond Gain; Kriegmair & Wulff; Chang & Jia; Kim et al.; Zheng & Yang | Four lines give a wide interval; line equals maker |
| 3. Zero-check rule | The estimator is known; this use was not found | First zero-check carry-over of learned memory scores, with the factor learned by leaving one line out | Ibrahim & Chen; Gravestock & Held; Reznik; Kveton et al.; RouteProfile; LRMF; Mondal et al.; memscore | Rests on an untestable assumption; a fixed factor may do as well; the edge fades fast |
| 4. Behavioural distance | The measures exist; forecasting score survival from them was not found | A memory-free forecast made before the swap, tested on held-out lines | CAPA; PhyloLM; Qiao et al.; Jalouzot et al.; Baek et al. | Collapses into the capability gap or the release label |

These verdicts rest on searches that found no counter-example. That shows only that nothing turned up, in a field that produced many related papers in 2026 alone.

## Eighteen small models fit two free Kaggle GPUs, but Gemma fights back

### The picture: four workshops, one set of shop rules

Think of four workshops, each with a row of mechanics hired in different years. A fair comparison gives every mechanic a similar toolbox, the same working hours and the same shop rules. For models, that means similar size, the same number precision, the same "thinking" setting and the same prompt format.

### The four lines and the traps in each

All four lines are confirmed on the makers' own pages. Each has four to seven versions small enough for one 16 GB GPU. Qwen has no small 3.6 or 3.8 model; those exist only at 27B and above ([Qwen3.5 README](https://github.com/QwenLM/Qwen3.5)). The suggested set below has 18 models, plus an optional nineteenth from Gemma.

| Line, step | Model (Hugging Face id) | Size (B) | Released | Licence | On a free T4 | Watch out |
|---|---|---|---|---|---|---|
| Qwen 1 | Qwen/Qwen1.5-1.8B-Chat | 1.84 | Feb 2024 | Research only, non-commercial | vLLM, 16-bit; about 24 requests at once | Older full attention uses more memory per request |
| Qwen 2 | Qwen/Qwen2-1.5B-Instruct | 1.54 | Jun 2024 | Apache-2.0 | vLLM, 16-bit | New attention design and tokenizer |
| Qwen 3 | Qwen/Qwen2.5-1.5B-Instruct | 1.54 | Sep 2024 | Apache-2.0 | vLLM, 16-bit | Same shape as Qwen2: only data and training changed |
| Qwen 4 | Qwen/Qwen3-1.7B | 2.03 | Apr 2025 | Apache-2.0 | vLLM, 16-bit | Thinks by default: switch it off |
| Qwen 5 | Qwen/Qwen3.5-2B | 2.27 | Mar 2026 | Apache-2.0 | vLLM, 16-bit, text-only flag | New architecture, tokenizer and vision all at once; T4 support untested |
| Gemma 1 | google/gemma-1.1-2b-it | 2.51 | Apr 2024 | Gemma Terms (licence acceptance needed) | vLLM, 16-bit | No system message allowed |
| Gemma 2 | google/gemma-2-2b-it | 2.61 | Jul 2024 | Gemma Terms | vLLM, 32-bit only | No system message; slower |
| Gemma 3 | google/gemma-3-1b-it | 1.00 | Mar 2025 | Gemma Terms | vLLM, 32-bit only | No 2B exists, so the size drops |
| Gemma 4 | google/gemma-3n-E2B-it | 5.44 raw ("2B effective") | Jun 2025 | Gemma Terms | vLLM, 16-bit (untested) | Raw size far above its label |
| Gemma 5 (option) | google/gemma-4-E2B-it | 5.12 raw (2.3 effective) | Mar 2026 | Apache-2.0 | Not in vLLM on a T4; use transformers or Ollama | A different serving engine is a confound |
| Granite 1 | ibm-granite/granite-3.0-2b-instruct | 2.63 | Oct 2024 | Apache-2.0 | vLLM, 16-bit | 4,096-token window |
| Granite 2 | ibm-granite/granite-3.3-2b-instruct (or 3.1) | 2.53 | Apr 2025 | Apache-2.0 | vLLM, 16-bit | Default prompt inserts today's date: always send your own |
| Granite 3 | ibm-granite/granite-4.0-micro | 3.40 | Oct 2025 | Apache-2.0 | vLLM, 16-bit | New tokenizer |
| Granite 4 | ibm-granite/granite-4.1-3b | 3.40 | Apr 2026 | Apache-2.0 | vLLM, 16-bit | Same shape as 4.0-micro (read from the config files; IBM does not say so) |
| Granite 5 | ibm-granite/granite-4.2-3b | 3.66 | Aug 2026 | Apache-2.0 | vLLM, 16-bit | Thinks by default: switch it off |
| Phi 1 | microsoft/phi-2 | 2.78 | Dec 2023 | MIT | vLLM, 16-bit; about 13 at once; plain-completion endpoint | Base model with no chat template; 2,048-token window |
| Phi 2 | microsoft/Phi-3-mini-4k-instruct | 3.82 | Apr 2024 | MIT | vLLM, 16-bit; about 8 at once | Older full attention |
| Phi 3 | microsoft/Phi-3.5-mini-instruct | 3.82 | Aug 2024 | MIT | vLLM, 16-bit; about 8 at once | Same shape as Phi-3-mini |
| Phi 4 | microsoft/Phi-4-mini-instruct | 3.84 | Feb 2025 | MIT | vLLM, 16-bit; about 24 at once | New 200K vocabulary |

Sizes come from the Hugging Face model API. Release dates come from the makers' pages: Qwen ([news list](https://github.com/QwenLM/Qwen3)), Gemma ([releases](https://gemma.ai/models/releases/)), Granite model cards such as [4.2-3b](https://huggingface.co/ibm-granite/granite-4.2-3b), and Phi model cards such as [phi-2](https://huggingface.co/microsoft/phi-2). The "at once" figures are estimates worked out from each model's configuration file, not measured memory peaks.

Five traps decide whether a "version effect" is real or an accident of setup:

1. **Thinking defaults differ.** Qwen3 thinks by default, Qwen3.5-2B does not, Granite 4.2 does, and Gemma 4 thinks only when asked ([Qwen3 card](https://huggingface.co/Qwen/Qwen3-1.7B); [Gemma 4 card](https://huggingface.co/google/gemma-4-E2B-it)). Switch thinking off everywhere. Otherwise the "version effect" is really a reasoning-mode effect.
2. **Phi-2 is not a chat model.** Its card says "The model has not undergone instruction fine-tuning" ([phi-2 card](https://huggingface.co/microsoft/phi-2)), so it could fail for reasons that have nothing to do with its line. Report every Phi result both with and without it.
3. **Prompt rules differ.** Gemma 1.1 and 2 reject a system message ("System role not supported") ([template](https://huggingface.co/unsloth/gemma-2-2b-it/blob/main/tokenizer_config.json)), so put the system text into the first user message. Granite 3.1–3.3 write today's date into a default system message ([template](https://huggingface.co/ibm-granite/granite-3.1-2b-instruct/blob/main/tokenizer_config.json)), so always send your own.
4. **Sizes are not perfectly matched.** Qwen stays at 1.5–2.3B and Phi near 3.8B. Gemma 3 has no 2B model, and the newer Gemma "E2B" models carry over 5B raw parameters. Size must be held fixed in the analysis.
5. **Licences differ.** Qwen1.5-1.8B is research-only and says its output may not be used "to improve any other large language model" ([licence](https://huggingface.co/Qwen/Qwen1.5-1.8B-Chat/blob/main/LICENSE)). None of the licences forbids publishing model outputs in a paper, but the paper should name each one.

### What free Kaggle gives, and where it breaks

Kaggle's default free GPU option is **"GPU T4 ×2"**: two separate T4 cards ([Kaggle CLI docs](https://github.com/Kaggle/kaggle-cli/blob/main/docs/kernels.md)) with 15,360 MiB of memory each ([benchmark](https://github.com/lyftium/vllm-serving-benchmark-t4)). The P100 was retired on 15 September 2026 ([Kaggle](https://www.kaggle.com/product-announcements/735239)). The L4, A100 and H100 in Kaggle's list are reserved for some competitions or for Kaggle staff. Third-party guides report a **12-hour cap per session** and a weekly allowance that "floats" around 30 hours, resetting at 00:00 UTC on Saturdays. Kaggle publishes no official figure, and its own documentation pages could not be read ([gpuperhour](https://gpuperhour.com/blog/free-cloud-gpus-and-credits); [fastero](https://fastero.com/blog/kaggle-gpu)). Internet access is off unless the notebook turns it on ([metadata docs](https://github.com/Kaggle/kaggle-cli/blob/main/docs/kernels_metadata.md)), and `kaggle kernels push` runs a notebook with no browser open.

The T4 has no native support for bfloat16, the 16-bit number format most new models ship in. If you ask vLLM for bfloat16 it refuses. By default it switches to float16 and logs a warning ([vLLM source](https://github.com/vllm-project/vllm/blob/v0.34.0/vllm/platforms/cuda.py)). That works for Qwen, Granite and Phi. Gemma breaks in three ways:

- **vLLM blocks float16 for Gemma 2 and 3**, citing "Numerical instability", so both fall back to 32-bit, which doubles their memory ([vLLM source](https://github.com/vllm-project/vllm/blob/v0.34.0/vllm/config/model.py)). Gemma 3's internal values overflow float16 on T4 cards ([Unsloth](https://unsloth.ai/docs/models/tutorials/gemma-3-how-to-run-and-fine-tune)).
- **Gemma-3-4B does not fit.** In 32-bit it needs about 16 GiB, more than one card holds, which is why the table uses Gemma-3-1B.
- **Gemma 4 "cannot currently run on Turing architecture GPUs" in vLLM.** This is an open bug ([issue #38918](https://github.com/vllm-project/vllm/issues/38918)). Gemma 4 therefore needs transformers or Ollama instead, and the paper must record that the serving engine and precision differ inside that one line.

### How to serve them, and how long it takes

The command `vllm serve <model> --dtype half` gives each model an OpenAI-style endpoint ([vLLM model registry](https://github.com/vllm-project/vllm/blob/v0.34.0/vllm/model_executor/models/registry.py)). The project's existing API-call code can therefore point at it unchanged. Run one model per card rather than splitting one model across both. The only vLLM version that people have publicly tested on Kaggle T4s is 0.18.0 ([vllm-t4-lab](https://github.com/Placidoe/vllm-t4-lab)); the current 0.34.0 is untested there. Phi-2 needs the plain-completion endpoint with its "Instruct: …\nOutput:" format, because vLLM refuses a chat request when a model has no chat template ([vLLM source](https://github.com/vllm-project/vllm/blob/v0.34.0/vllm/renderers/hf.py)). Pin each model's exact Hugging Face revision, and keep the number of parallel requests the same for every model in a line.

The design keeps the cost low. If every model reads the same frozen items, each model only has to be run once. Every one of the **306 ordered pairs** among 18 models then becomes a table lookup: 64 pairs inside lines and 242 across (our count). Cost grows with the number of models, not the number of pairs. Because the pairs share models, they are not independent, so the analysis must account for which source model and which target model each pair uses.

Here is a rough estimate (our arithmetic, not a measurement). The run uses about 1,200 probe tasks per task family: 400 for each of three stream orders, the size the project's earlier power simulation recommended. Each task is run with each of four candidate items, plus once with no item, and every run is repeated twice. That comes to about 12,000 calls per model per task family, or about 430,000 calls for 18 models and two task families.

The only measured Kaggle speed is **about 950 output tokens a second for Qwen2.5-1.5B at 16 parallel requests on one T4**, and 68 tokens a second for one request at a time ([vllm-t4-lab](https://github.com/Placidoe/vllm-t4-lab)). There are no T4 measurements for 3–4B models, nor for any Phi, Granite or Gemma model. A cautious two to four short answers a second per card, averaged over all model sizes, gives 15–30 hours on both cards. One hour on the two-card machine costs one quota hour ([fastero](https://fastero.com/blog/kaggle-gpu)). Doubling the total to allow for smoke tests and failed runs gives **about 30–60 quota hours, which is one to two weeks of one account's floating allowance**. Each job must save its progress and finish in about 11 hours to stay inside the session cap.

Two open questions remain. The project's one-item probe prompts were measured at about 740–975 tokens in its earlier plan. That fits even Phi-2's 2,048-token window, with room for a short answer. Whether small 1.5–4B models gain anything from memory on the chosen tasks is unknown, and the pilot must check that for every model before anything else.

**What to do.** This week, give every model on Kaggle a 20-prompt smoke test in its stated precision, with thinking off and the fixed prompt format, and drop or flag any that fail. Settle the Gemma choice after that test. Keep Gemma 4 only if it runs stably in transformers at 16-bit. Otherwise end the Gemma line at Gemma 3n and say so in the paper.

## Reviewers will attack size, distance and predictability; build the answers in now

### The picture: the inspector who has seen many garages

A tough inspector will ask three questions. Do your findings hold for big garages, not just small ones? How exactly do you measure "how far apart" two mechanics are? And didn't everyone already know that newer apprentices differ more? Reviewers of the closest papers asked exactly these questions.

| Objection | Where reviewers already said it | The answer to build in |
|---|---|---|
| "Small open models only. Will it hold for the big models people actually swap?" | "most experiments are conducted on relatively small models … test different versions of gpt-4o" ([review](https://openreview.net/forum?id=fwHVclv0ij&noteId=htXCuXGTNd)); "limited to the Qwen3 model series" ([review](https://openreview.net/forum?id=HpOsmqXojJ&noteId=1XEVHJFVDI)) | Four lines, not one; size held fixed in the analysis; one larger or paid model pair as an outside check; open weights presented as a reproducibility strength |
| "Version distance is ill-defined, and lineage is already known" | "which generation they are from … already known" ([PhyloLM review](https://openreview.net/forum?id=rTQNGQxm4K&noteId=ysd2GWVrQF)); "lacks a clear statistical comparison between 'same family' and 'different family' pairs" and "training data overlap" ([CAPA review](https://openreview.net/forum?id=3Z827FtMNe&noteId=TtKSQfqOJR)); "account for model size" ([CAPA review](https://openreview.net/forum?id=3Z827FtMNe&noteId=hOrW0PWgxM)) | Several distance measures (steps, months, behaviour); capability gap and size taken out; a within-line vs cross-line comparison at matched distance, written down in advance |
| "Predictable, or not new" | "results are mostly predictable and the novelty is fairly limited", from an ICML 2025 area chair who still recommended acceptance ([meta-review](https://openreview.net/forum?id=MpjtvkvXDo&noteId=UcGMbzWjoL)); per-example scores already transfer across sizes and families ([LESS](https://arxiv.org/abs/2402.04333)) | Classic estimators included as named arms; lead with a decision result; look for where the obvious expectation fails |
| "One task family, or synthetic data" | "it only uses one model" ([LifelongAgentBench review](https://openreview.net/forum?id=MYqAKKsjF9&noteId=Wqod02lcIQ)); "LLM-induced … biases during data generation" ([review](https://openreview.net/forum?id=HpOsmqXojJ&noteId=1XEVHJFVDI)) | Two task families whose answers are checked by a program |
| "No error bars or seeds" | "absence of multiple seeds, standard deviations, and error bars" ([MemRL meta-review](https://openreview.net/forum?id=FBlIaSaXnX&noteId=8sgIUY6dXU)) | At least 3 seeds × 2 stream orders; an interval on every number; predictions frozen in a file before the runs |
| "Stronger models don't need memory" | "stronger LLMs might answer correctly without genuine on-the-fly learning" ([review](https://openreview.net/forum?id=DT7JyQC3MR&noteId=uR3qANeEDh)) | A no-memory control on every model, with a minimum memory gain required before a model counts |

**Small models are acceptable if the limit is stated.** ICML 2025 accepted a contamination study run at only 1B and 8B, with a reviewer noting the limit was "acknowledged in the paper" ([review](https://openreview.net/forum?id=MpjtvkvXDo&noteId=jEedE17dxP)). Another reviewer wrote of a study limited to 7–8B models, "I believe this is ok" ([review](https://openreview.net/forum?id=1CK1kuH1he&noteId=pMktU66C9s)). An ICML 2025 area chair praised open weights for increasing "accessibility and encourages reproducibility" ([meta-review](https://openreview.net/forum?id=cve4NOiyVp&noteId=6PWHg951YI)).

The reproducibility case is strong because paid models drift. "Providers replace their models within months" ([Same Papers](https://arxiv.org/abs/2611.20608)), and on API gateways "a model name is a routing label rather than a verified snapshot" ([OpenReview](https://openreview.net/forum?id=fmDOTwVZes)). Only open lines give a complete version history. But never write "anyone can rerun it forever". No source shows that Hugging Face checkpoints are permanent, so write "as long as the pinned checkpoints stay available", and archive the score tables. For the outside check, Beyond Gain's released per-item scores under Qwen3.6-27B and Qwen3.8-27B cost nothing ([Beyond Gain](https://arxiv.org/html/2610.32322)). A paid pair of larger models costs money and needs your decision.

**Answering "predictable" needs a finding that is not obvious.** There are three candidates, and the earlier record hints at each:

- **Loss may not grow evenly with distance.** UpgradeBench's retention swung from 0.932 to 0.414 to 0.578 over three successive steps ([UpgradeBench](https://arxiv.org/html/2608.20918)), and newer Qwen versions were not consistently better judges ([Yu et al.](https://arxiv.org/abs/2610.09820)).
- **Upgrades and downgrades may differ.** A scorer moved from a 4B to an 8B model saw "seen" success fall from 81.43 to 68.57, while the reverse move helped ([rebuttal](https://openreview.net/forum?id=I90KA7L0dd&noteId=ddUhb5BapP)). Model-written memory notes gained 9.91 points or lost 13.28 depending on direction ([Goyal & Ray](https://arxiv.org/html/2609.05339)).
- **A cross-line pair could beat a within-line pair** at the same distance.

Writing predictions down in advance makes any of these findings count. Explore on the Qwen line first. Then write down predictions for Gemma, Granite and Phi, and time-stamp them, before running those lines. A correct prediction made in advance is evidence; a pattern found afterwards looks like hindsight.

**Four more design rules close the remaining gaps.**

- **Show the scores are pinned down before the swap.** A November paper found per-memory values pinned down in 0 of 14 benchmarks, across 55 configurations, when only top-k retrieval logs were available ([arXiv 2611.03289](https://arxiv.org/abs/2611.03289)). Report the share of items whose old score clearly differs from the default.
- **Report order and level separately**, because "Same Papers" shows they can split.
- **Run both directions of every pair**, plus a rerun of the same model as the noise floor.
- **Define "value kept" between a floor and a ceiling**, as UpgradeBench does. Zero means no better than ignoring the scores. One means as good as the new model learning the scores itself.

## ICML on 22 January, with NeurIPS E&D and TMLR as the safety net

### Venues and dates

| Venue | Fit | Dates (posted unless marked) | Notes |
|---|---|---|---|
| ICML 2027, "Datasets, Evaluations and Benchmarks" type | High | OpenReview account suggested by 31 Dec 2026; submissions open 8 Jan; abstract 14 Jan; paper 22 Jan; first-stage rejections 23 Feb; decisions 3 May; conference 2–4 Aug ([Dates](https://icml.cc/Conferences/2027/Dates)) | This type does "not need to introduce a new learning algorithm" ([Contribution Types](https://icml.cc/Conferences/2027/ContributionTypes)) |
| NeurIPS 2027 Evaluations & Datasets | Highest by its charter | Not posted; 2026 had abstract 4 May, paper 6 May ([2026 CFP](https://neurips.cc/Conferences/2026/CallForEvaluationsDatasets)) | Welcomes "negative results, critical analyses"; wants hosted code and data with Croissant metadata at submission |
| TMLR | High for a measurement paper | Rolling ([policies](https://jmlr.org/tmlr/editorial-policies.html)) | Asks whether claims are "supported by accurate, convincing and clear evidence"; cannot be under review at the same time as ICML |
| ICLR 2028 | High; same readers as the closest rivals | Not posted; about late September 2027 going by past years ([iclr.cc](https://iclr.cc/)) | ICLR 2027 decisions arrive 16 Dec 2026 |
| ACL / EMNLP 2027 via ARR | Medium | ACL "January, 2027"; EMNLP not listed ([ARR](https://aclrollingreview.org/dates)) | A January submission would overlap ICML review, which is forbidden |
| COLM 2027, AAAI-28, KDD 2027 D&B cycle 2 | Medium | None posted ([COLM](https://colm.cc/); [AAAI](https://aaai.org/conference/aaai/aaai-27/); [KDD](https://kdd2027.kdd.org/datasets-and-benchmarks-track-call-for-papers/)) | Later options |

The order follows from the dates:

1. Submit to ICML on 22 January.
2. If ICML rejects at the first stage on 23 February, the nearby options are COLM 2027 (deadline not yet posted) or a workshop whose papers are not formally published.
3. If ICML rejects at the final decision on 3 May, go to NeurIPS E&D if its 2027 deadline falls after 3 May (in 2026 it fell three days later). Otherwise go to ICLR 2028 or TMLR.

Prepare the NeurIPS materials (hosted code, Croissant metadata) before the ICML deadline, so a switch in May costs nothing.

### ICML rules that can sink a sound paper

**An AI Disclosure Statement is mandatory.** It sits outside the page limit and must say how AI was used and how its output was checked ([GenAI policy](https://icml.cc/Conferences/2027/UseOfGenerativeAIByAuthors)). This project's use of AI agents for code, analysis and drafting must be disclosed in full.

**Three things lead to rejection without review**: hallucinated references, placeholder abstracts, and "large differences" between the abstract filed on 14 January and the final paper. "Low-quality AI-generated content (AI slop)" can sink all of an author's submissions ([Peer Review Ethics](https://icml.cc/Conferences/2027/PeerReviewEthics)). The author list freezes at the abstract deadline ([CFP](https://icml.cc/Conferences/2027/CallForPapers)).

The submission limit is unclear. A 17 November change to the call for papers says an author who does not qualify as a reviewer may appear on "at most 2 submissions". The reciprocal-reviewing page still says "at most one submission in which no author is an eligible reciprocal reviewer" ([Reciprocal](https://icml.cc/Conferences/2027/ReciprocalReviewingAndSubmissionQuotas)). If no author has a paper at a listed venue, ask the chairs. ICML also promised process updates "by late November", which had not appeared by 26 November.

### A seven-week calendar

| Dates | Work | Done when |
|---|---|---|
| 26 Nov – 2 Dec | Smoke-test every model on Kaggle; pin revisions; open OpenReview profile; read the HEIRS PDF; re-read the ICML pages after the late-November update | Every model answers 20 test prompts in its stated precision |
| 3 – 9 Dec | Pilot on the Qwen line, one task family: memory gain per model, rerun agreement, share of scores pinned down | Go or no-go |
| 10 – 13 Dec | Write and time-stamp the predictions for Gemma, Granite and Phi | Prediction file committed |
| 14 – 28 Dec | Full runs on all lines and two task families, plus behaviour probes; read the ICLR decisions on 16 Dec | All result tables filled |
| 29 Dec – 4 Jan | Distance curves, compounding test, three-way split, zero-check rule, outside check; freeze the numbers | Every number final |
| About 5 Jan | Repeat the novelty search | Related-work table final |
| 5 – 13 Jan | Write the abstract, introduction and Figure 1 first | Draft read by one outside reader |
| 14 Jan | Final abstract and author list | Registered |
| 15 – 22 Jan | Check every reference by hand; AI Disclosure Statement; anonymous code; submit | Submitted |

Four weeks of runs at about 30 quota hours a week gives roughly 120 hours, about twice the cautious estimate above. The calendar is tight, and the real limit is engineering time, not GPU time.

## Conclusion

The paper's strongest result is unlikely to be "scores fade with distance", which every reviewer expects. It is more likely to be *where* the fading sits. The four lines contain matched natural experiments: steps that keep the model's shape and change only training, next to steps where the maker rebuilt the architecture and tokenizer. If the loss bunches at the rebuilds and a line keeps a floor that never fades, the practical rule for anyone running an agent becomes "read the release notes, not the version number". A same-shape update would need few or no checks, while a rebuilt model would need a stated recheck budget. That is a decision rule a builder can use on swap day, and no earlier paper could produce it, because none walked more than one or two steps.

The four ideas pay off together, not separately. On its own, each one is a known tool applied to a new object, and reviewers will say so. Together they describe one quantity: the noise-corrected agreement between old and new scores. It can be forecast before the swap from behaviour, multiplied along a chain of releases, split into what the line shares and what each version owns, and turned into a day-one shrink factor. Because one shared factor cannot reorder items, the honest end product is a budget, not a winning rule: how many fresh checks a swap of a given distance needs before the old scores can be trusted. Free Kaggle hardware makes the whole thing repeatable by any reader at no cost, including the token probabilities behind the behaviour measure. That turns "small models only" from a weakness into the reason the result can be checked. Gemma is the line where "same conditions for every model" breaks first, so treat it as the stress test rather than the showcase.
