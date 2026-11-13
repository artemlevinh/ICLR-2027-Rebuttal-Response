# ICLR-2027-Rebuttal-Response


**Paper: Evidence-Grounded Bayesian Optimization with Collective LLM Reasoning**

---

# Response to Reviewer 1

We sincerely thank the reviewer for the careful and detailed assessment. We appreciate the positive comments and thoughtful suggestions, which have helped us identify areas for improvement. We acknowledge that several methodological choices were not sufficiently explained in the original submission. In the revised manuscript, we have clarified the motivation, implementation, theoretical scope, and experimental evidence. We address each concern individually below.

### 1. Figure 1 and the overall workflow

We agree that Figure 1 was too dense. We have redesigned it around four steps: (i) gather structured DoE evidence, (ii) obtain evidence-grounded region proposals from LLMs, (iii) aggregate the proposals, and (iv) use them to guide GP-based BO. Algorithmic details now appear in the method text rather than in the overview figure.

**See revised Figure 1 and Section 2.1.**

### 2. Notation, methodological clarity, and interpretation of Theorem 1

We respectfully clarify that Theorem 1 already provides a probabilistic characterization of the robustness of our aggregation procedure. The probability bound quantifies the likelihood that the aggregated proposal remains within a specified neighborhood of the target, under the stated assumptions. We believe this provides theoretical support for our aggregation strategy, although we acknowledge that the original manuscript did not sufficiently explain the intuition behind the bound or its practical implications for optimization.

We also agree that the methodological presentation can be simplified. In the revised manuscript, we have clarified the notation, added an intuitive discussion of Theorem 1 and its assumptions, and streamlined the pseudocode to emphasize the main algorithmic steps. Secondary implementation details have been moved to the appendix to make the modeling choices and overall reasoning easier to follow.

**See revised Sections 2–3 and Appendix B.**


### 3. Choice of the DoE fraction $\rho_{\mathrm{DoE}}$

We agree that $\rho_{\mathrm{DoE}}$ involves a trade-off between gathering sufficient initial evidence and preserving evaluations for sequential BO. Our choice of $\rho_{\mathrm{DoE}}=0.3$ is empirically motivated, based on experimental comparisons across different DoE fractions under the same total evaluation budget. We have added these sensitivity results to justify this choice.

**See revised Section 4.5.**

We also appreciate the suggestion of adaptive budget allocation and consider it a promising direction for future work.

### 4. Definition of $\mathcal{L}_{j,s}$ and normalization

We thank the reviewer for pointing out this ambiguity. The parameter $L_{i,s}$ represents the scalar trust-region size associated with anchor $x_i$ at iteration $s$, as defined in our appendix. We agree that this definition should also appear in the main text. We have therefore clarified the notation and added an explicit definition of the numerical-coordinate normalization $\phi_j$ used in constructing the trust regions.

**See revised Section 2.4 and the notation appendix.**

### 5. Why does LLM influence decrease over time?

We thank the reviewer for this important question. Our design follows the early-LLM/late-GP principle of LLINBO (Chang et al., 2025), already cited in Section 2.4. The motivation is not that LLMs are inherently more reliable initially, but that their guidance can complement GP-based search when observations are limited. Our DoE stage further grounds these recommendations in statistical evidence.

To strengthen this justification, we have added a theoretical analysis inspired by LLINBO's regret-bound framework, explicitly accounting for our scheduling assumptions, together with a sensitivity analysis of the decay parameters.

**See revised Sections 3 and 4.5.**

### 6. Stronger single LLMs, aggregation, and the distance metric

We agree that our original seven-model comparison with one GPT-4o Mini agent cannot separate model strength, proposal count, diversity, and inference cost.

However, we would like to clarify our central motivation.

**Our objective is not necessarily to outperform the strongest individual LLM on every task, but to avoid assuming that the best-performing LLM is known beforehand.**

In black-box optimization, different LLMs may perform well on different objective functions, and identifying the most reliable model in advance may itself require costly evaluations. Our framework therefore aims to reduce dependence on any particular LLM through evidence-grounded aggregation.

To assess this motivation, we compare:

1. Individual LLM proposals, including strong constituent models.
2. Seven independently sampled proposals from one strong LLM, using the same aggregation rule.
3. Seven proposals from heterogeneous LLMs, using projected-mean/medoid aggregation.
4. A uniformly selected proposal from the same seven-proposal pool.

We keep prompts, DoE evidence, random seeds, and objective evaluation budgets aligned, while reporting inference costs separately.

We also examine whether individual-model rankings vary across benchmarks and whether aggregation reduces variability and downside risk.

**See revised Section 4.3.**

Regarding the evaluation metrics, we distinguish distance to the nearest global optimizer, initialization regret, and end-to-end normalized simple regret. These metrics capture different aspects of initialization quality and optimization performance.

**See revised Section 4.1 for the metric definitions.**

### 7. Comparison with a strong LLM as a direct optimizer

We thank the reviewer for this valuable suggestion. We would like to emphasize that a central motivation of our framework is that the strongest LLM for a given optimization problem cannot be identified beforehand, and its reliability may vary across tasks. Rather than relying on a single model, our approach aggregates multiple LLM recommendations to reduce model-selection risk.

Nevertheless, we agree that directly using a strong LLM for sequential optimization is an important baseline. We have therefore added an experiment comparing direct strong-LLM optimization with our framework under the same objective evaluation budget.

**See revised Section 4.3.**

### 8. Evaluation on heterogeneous decision variables

We would like to respectfully clarify that our experiments are not limited to continuous domains. The FCNet Naval hyperparameter optimization experiment (Section 3) involves nine hyperparameters, including categorical, integer-valued, and numerical variables. Our x264 software configuration experiment (Section 4) further evaluates optimization over a constrained discrete search space.

We agree that additional evidence would strengthen the generality of our approach. We have therefore added another benchmark involving heterogeneous decision variables to further evaluate its performance and robustness.

**See revised Section 4.6 for the additional benchmark and results.**

### 9. Early performance and later catch-up

We thank the reviewer for this observation. The initial slower performance mainly reflects the evaluation budget allocated to DoE, which aims to capture the objective landscape rather than immediately exploit promising points. The resulting statistical evidence helps identify promising subregions, enabling more efficient sequential optimization and explaining why our method catches up and subsequently improves upon the baselines.

To better illustrate this mechanism, we have added visualizations showing how DoE captures the objective landscape and guides the identification of promising search regions.

**See the new visualizations in the revised Appendix.**

### 10. Meaning of "warm-start GP"

We thank the reviewer for pointing out this ambiguity. The warm-start GP baseline simply uses LLM-generated initial evaluation points instead of random initialization, then proceeds with standard GP-based BO. We have clarified this definition in the revised manuscript.

### 11. DoE followed by direct search without LLM reasoning

We thank the reviewer for this valuable suggestion. We agree that comparing our framework with DoE initialization followed by conventional direct search would help isolate the contribution of collective LLM reasoning. We have therefore added a baseline using the same initial DoE observations, followed by pattern search without LLM guidance, under the same total evaluation budget.

**See revised Section 4.4 for the additional experiment.**

### 12. Presentation and organization

We appreciate these concrete suggestions.

We have simplified Figure 1, moved secondary implementation details to the appendix, standardized notation, and reorganized the paper so that the motivation precedes the mathematical details.

We have also checked figure references and consistency between theoretical definitions, propositions, and proofs.

**See revised Sections 2–3 and the appendices.**

### Closing remarks

We sincerely thank the reviewer again for the detailed and constructive feedback.

We hope the revised explanations and additional experiments better clarify our central contribution: **using evidence-grounded aggregation to reduce reliance on an individual LLM whose task-specific reliability is unknown beforehand, while retaining actual objective evaluations as the foundation of Bayesian optimization.**

# Response to Reviewer 5LFU

We sincerely thank the reviewer for the constructive assessment and thoughtful suggestions. We appreciate the recognition of the practical motivation and empirical promise of our framework. We agree that further clarification and controlled experiments can help distinguish the contributions of DoE, LLM reasoning, and aggregation. We address each concern below.

### W1. Are the improvements due to model strength, more proposals, or heterogeneity?

We thank the reviewer for this important observation. We agree that comparing seven heterogeneous LLMs with a single GPT-4o Mini does not fully isolate the effects of model strength, proposal count, and aggregation.

However, we would like to emphasize that **our objective is not necessarily to outperform the strongest individual LLM on every task, but to avoid assuming that the best-performing LLM can be identified beforehand.** Different LLMs may perform well on different optimization problems, making reliance on a single model potentially risky. Our collective reasoning approach aims to reduce this model-selection risk by aggregating recommendations from multiple models.

To better evaluate this motivation, we have added controlled comparisons involving:

1. Individual strong LLMs.
2. Seven independently sampled proposals from one strong LLM.
3. Seven heterogeneous LLM proposals with the same aggregation.
4. Random selection from the same seven-proposal pool.

These experiments use matched DoE evidence and objective evaluation budgets, with inference costs reported separately. We also examine individual-model rankings and the robustness of aggregated proposals across benchmarks.

**See revised Section 4.3 for the additional experiments.**

### W2. Why use an LLM when the DoE statistics are already interpretable?

We appreciate this thoughtful question. We agree that DoE statistics, including main effects and interactions, can be used directly to guide optimization. However, **our motivation for involving LLMs is to interpret statistical evidence jointly with variable descriptions and identify promising search subregions**, rather than relying solely on predefined statistical selection rules.

Nevertheless, we agree that the additional contribution of LLM interpretation should be demonstrated experimentally. We have therefore added a statistical-only baseline that uses the same DoE evidence to select search regions without LLM reasoning, followed by the same GP-based optimization procedure.

We also compare our method with DoE initialization followed by conventional pattern search to further isolate the value of LLM guidance.

**See revised Section 4.4 for these additional comparisons.**

### W3. What is new beyond combining established components?

We thank the reviewer for raising this important question. We agree that combining established techniques alone does not necessarily constitute a methodological contribution.

Our contribution lies in **using statistical evidence from target-system evaluations to guide collective LLM reasoning under uncertainty about individual LLM reliability**. Specifically, DoE helps characterize the objective landscape and identify promising subregions, collective reasoning reduces dependence on any single LLM, and sequential GP-based optimization increasingly relies on actual objective observations.

To distinguish these contributions, we have expanded our component ablations to compare DoE-only optimization, statistical-only region selection, single-LLM guidance, and collective LLM aggregation under matched evaluation budgets.

We have also added visualizations illustrating how DoE evidence guides search-region identification, along with a clearer theoretical discussion of the aggregation mechanism.

**See revised Section 4.4 for the ablation experiments, Section 3 for the theoretical discussion, and the revised Appendix for the visualizations.**

### Closing Remarks

We sincerely thank the reviewer again for these valuable suggestions. We believe the additional controlled experiments and methodological clarifications better establish the respective contributions of statistical evidence, LLM reasoning, and aggregation.

Importantly, our central motivation is to provide robust optimization guidance without requiring prior identification of the most reliable LLM for each task.


# Response to Reviewer wr5s

We sincerely thank the reviewer for the careful assessment and constructive suggestions. We appreciate the positive comments on our integration of statistical evidence and collective LLM reasoning, as well as the detailed feedback on experimental fairness and theoretical interpretation. We address each concern below.

### W1. Is heterogeneous multi-LLM aggregation justified by the baseline?

We thank the reviewer for this important observation. We agree that comparing seven heterogeneous LLMs with one GPT-4o Mini does not fully isolate the effects of model strength, proposal count, and aggregation.

However, we would like to clarify that **our objective is not necessarily to outperform the strongest individual LLM, but to reduce dependence on a single model when its reliability for a new optimization problem is unknown beforehand.** The best-performing model may vary across tasks, making it difficult to select one reliably in advance.

To better support this motivation, we have added controlled experiments comparing individual strong LLMs, homogeneous seven-proposal ensembles, heterogeneous seven-LLM ensembles, and random proposal selection. We maintain the same DoE evidence and objective evaluation budgets while reporting inference costs separately.

We additionally examine whether model rankings vary across benchmarks and whether aggregation improves robustness and reduces poor initialization outcomes.

**See revised Section 4.3 for the additional experiments.**

### W2. Does the Rastrigin design use knowledge of the global optimum?

We sincerely thank the reviewer for identifying this important concern. We agree that knowledge of the global optimum should not influence the initialization procedure in a black-box optimization setting.

We acknowledge that our original explanation of the Rastrigin DoE design referred to the known optimum at the origin, which could create concerns about benchmark-specific information influencing initialization.

To address this issue, we have revised the experimental protocol to use a benchmark-independent DoE selection rule that does not rely on knowledge of the global optimum. We have also repeated the corresponding experiments under this initialization procedure to ensure a fair comparison.

**See revised Section 4.6 for the updated experiments.**

### Q1. Empty Appendix A.6

We apologize for this editorial oversight. We have corrected the incomplete appendix section and checked the revised manuscript for similar issues.

**See the revised Appendix.**

### Q2. Incorrect Figure 3(a) reference

We thank the reviewer for pointing out this error. We have corrected the figure reference and checked the consistency of the remaining figure numbers and captions.

**See revised Figure 3.**

### Q3. Inconsistent notation

We agree that consistent notation is essential for understanding the methodology and theoretical analysis. We have standardized the notation for search regions, decay parameters, and related quantities throughout the manuscript.

We have also simplified the pseudocode and clarified the definitions to improve readability.

**See revised Sections 2–3 and the Appendix.**

### Q4. Do the aggregation bounds imply BO robustness or regret guarantees?

We appreciate this important theoretical question. We would like to clarify that **our theoretical results establish robustness properties of proposal aggregation under specified assumptions, rather than directly providing an end-to-end regret guarantee for the entire BO procedure.**

In particular, Theorem 1 shows that when a sufficient majority of proposed regions lie close to a reference region, the medoid remains within a bounded distance of that reference. Its probability bound further requires independence assumptions, which are not guaranteed simply by using heterogeneous LLMs.

These results provide a theoretical explanation for the robustness of aggregation, but translating them into optimization performance requires additional assumptions relating proposal proximity to objective values.

For example, if $f$ is $L$-Lipschitz continuous and an aggregated candidate satisfies $\|x_{\mathrm{agg}}-x^*\|\leq\epsilon$ for a global optimizer $x^*$, then

$$
f(x_{\mathrm{agg}})-f(x^*)\leq L\epsilon.
$$

We agree that the original manuscript could better explain this distinction. We have therefore expanded the theoretical discussion, clarified the assumptions and interpretation of the probability bounds, and distinguished proposal-level robustness from overall optimization performance.

We have also strengthened the discussion of the LLM-to-GP transition, drawing on the motivation of LLINBO (Chang et al., 2025), while explicitly distinguishing our scheduling assumptions from those required by LLINBO's regret results.

The practical contribution of aggregation is further evaluated through the controlled experiments.

**See revised Section 3 and Appendix B for the theoretical analysis, and Sections 4.3–4.4 for the experimental evidence.**

### Closing Remarks

We sincerely thank the reviewer again for the thoughtful and constructive feedback. We believe the additional experiments, corrected initialization protocol, and clearer theoretical discussion strengthen the manuscript and provide a more precise interpretation of our contributions.

Our central objective remains to develop an evidence-grounded optimization framework that reduces reliance on any individual LLM while using actual objective evaluations to guide sequential optimization.
