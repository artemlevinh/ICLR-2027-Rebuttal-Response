# ICLR-2027-Rebuttal-Response


# ICLR 2027 Author Rebuttal
**Paper: Evidence-Grounded Bayesian Optimization with Collective LLM Reasoning**

---

# Response to Reviewer 1

We sincerely thank the reviewer for the careful and detailed assessment. We appreciate the positive comments and thoughtful suggestions, which have helped us identify areas for improvement. We acknowledge that several methodological choices were not sufficiently explained in the original submission. In the revised manuscript, we have clarified the motivation, implementation, theoretical scope, and experimental evidence. We address each concern individually below.

### 1. Figure 1 and the overall workflow

We agree that Figure 1 was too dense. We have redesigned it around four steps: (i) gather structured DoE evidence, (ii) obtain evidence-grounded region proposals from LLMs, (iii) aggregate the proposals, and (iv) use them to guide GP-based BO. Algorithmic details now appear in the method text rather than in the overview figure.

**See revised Figure 1 and Section 2.1.**

### 2. Notation, motivation, and theoretical interpretation

Thank you for noting definitions and explanations that were missing or introduced too late. We have clarified the variable metadata (including $\mathrm{type}_j$ and $\mathrm{desc}_j$), the DoE summary, region representation, aggregation, and GP update.

We also distinguish a bound on proposal aggregation from a guarantee on end-to-end BO regret. Our theoretical claims hold only under their stated assumptions; a distance guarantee implies an objective-value guarantee only with additional regularity conditions.

**See revised Sections 2–3 and Appendix B.**

### 3. Choice of the DoE fraction $\rho_{\mathrm{DoE}}$

We agree that this choice creates a trade-off: a larger initial design provides richer evidence but leaves fewer evaluations for sequential BO. Our current method uses a predetermined allocation, not an adaptively optimal allocation.

We clarify its selection rule and provide a sensitivity comparison across DoE fractions under the same total evaluation budget.

**See revised Section 4.5 [insert tested fractions and findings].**

Adaptive budget allocation remains an interesting extension, but we do not claim to solve it here.

### 4. Definition of $\mathcal{L}_{j,s}$ and normalization

We apologize for the ambiguity. We now define $\mathcal{L}_{j,s}$ at first use and specify the mapping between original decision-variable values and the representation used for proposal generation and aggregation.

We separately define input normalization and normalized regret to avoid conflating them.

**See revised Section 2.2 and Appendix A [verify definitions against code].**

### 5. Why does LLM guidance decay over time?

Our intention is not to assume that LLMs are more accurate early in optimization. Rather, early GP guidance is based on relatively few objective observations, so DoE-grounded proposals can provide a complementary source of information.

As verified evaluations accumulate, we increasingly favor the observation-driven GP.

We have clarified this rationale and examine sensitivity to the decay schedule.

**See revised Section 2.4 and Section 4.5 [insert results].**

### 6. Stronger single LLMs, aggregation, and the distance metric

We agree that our original seven-model comparison with one GPT-4o Mini agent cannot separate model strength, proposal count, diversity, and inference cost.

However, we would like to clarify our central motivation.

**Our objective is not necessarily to outperform the strongest individual LLM on every task, but to avoid assuming that the best-performing LLM is known ex ante.**

In black-box optimization, different LLMs may perform well on different objective functions, and identifying the most reliable model in advance may itself require costly evaluations. Our framework therefore aims to reduce dependence on any particular LLM through evidence-grounded aggregation.

To assess this motivation, we compare:

1. Individual LLM proposals, including strong constituent models.
2. Seven independently sampled proposals from one strong LLM, using the same aggregation rule.
3. Seven proposals from heterogeneous LLMs, using projected-mean/medoid aggregation.
4. A uniformly selected proposal from the same seven-proposal pool.

We keep prompts, DoE evidence, random seeds, and objective evaluation budgets aligned, while reporting inference costs separately.

We also examine whether individual-model rankings vary across benchmarks and whether aggregation reduces variability and downside risk.

**See revised Section 4.3 [insert numerical results].**

Regarding the evaluation metrics, we distinguish distance to the nearest global optimizer, initialization regret, and end-to-end normalized simple regret. These metrics capture different aspects of initialization quality and optimization performance.

**See revised Section 4.1 for the metric definitions.**

### 7. A strong LLM as the direct sequential optimizer

We agree that this is an important alternative to region-level guidance.

Our framework differs from direct LLM optimization by using structured DoE evidence, aggregating region proposals, and relying on actual objective evaluations to update the GP.

Nevertheless, these methodological differences alone do not establish superior performance.

We therefore include a baseline in which a strong LLM observes the same available objective history and directly proposes subsequent evaluation points without our collective aggregation stage.

The objective evaluation budget is matched, and inference cost is reported separately.

**See revised Section 4.3 [insert result].**

### 8. Evaluation on heterogeneous decision variables

We agree that continuous synthetic benchmarks alone cannot establish effectiveness across general mixed-variable domains.

These benchmarks allow controlled assessment of optimization performance, but they do not fully represent practical problems involving categorical, integer, and continuous variables.

We now clarify the scope of the synthetic experiments and include **[insert verified mixed-variable benchmark and results, if completed]**.

**See revised Section 4.6.**

We have also narrowed the claims where necessary to avoid generalizing beyond the evaluated settings.

### 9. Early performance and later catch-up

Thank you for highlighting this behavior.

A possible explanation is that our DoE stage allocates evaluations toward structured exploration before sequential exploitation. This may initially trail methods that identify a favorable point quickly, while potentially providing better information for subsequent GP-based search.

However, we agree that this explanation should be supported empirically rather than inferred solely from the regret curves.

We therefore separate initialization and sequential-stage performance and compare regret trajectories under equal total objective evaluation budgets.

**See revised Section 4.5 [insert results].**

### 10. Meaning of "warm-start GP"

We apologize for not defining this term explicitly.

A warm-start GP refers to a Gaussian Process fitted using the actual objective evaluations collected during the initial DoE stage, before subsequent BO iterations.

Importantly, LLM-generated proposals are used as search guidance, not as synthetic objective observations or pseudo-labels for GP training.

We have clarified the initialization and update procedures.

**See revised Sections 2.3–2.4.**

### 11. DoE followed by search without LLM reasoning

We agree that this baseline is necessary to distinguish the contribution of DoE initialization from the contribution of collective LLM reasoning.

We therefore include a baseline that uses the same DoE observations, followed by conventional BO without LLM-generated region proposals.

We also distinguish this comparison from an LLM-free statistical region-selection baseline.

All variants receive the same objective evaluation budget.

**See revised Section 4.4 [insert results].**

### 12. Presentation and organization

We appreciate these concrete suggestions.

We have simplified Figure 1, moved secondary implementation details to the appendix, standardized notation, and reorganized the paper so that the motivation precedes the mathematical details.

We have also checked figure references and consistency between theoretical definitions, propositions, and proofs.

**See revised Sections 2–3 and the appendices.**

### Closing remarks

We sincerely thank the reviewer again for the detailed and constructive feedback.

We hope the revised explanations and additional experiments better clarify our central contribution: **using evidence-grounded aggregation to reduce reliance on an individual LLM whose task-specific reliability is unknown beforehand, while retaining actual objective evaluations as the foundation of Bayesian optimization.**

---

# Response to Reviewer 5LFU

We sincerely thank the reviewer for the constructive assessment and for recognizing the practical motivation and empirical promise of our framework.

We agree that the original experiments did not sufficiently separate the contributions of DoE evidence, LLM interpretation, and aggregation. We address the three main concerns below.

### W1. Are the improvements due to model strength, more proposals, or heterogeneity?

We agree that comparing seven heterogeneous LLMs with a single GPT-4o Mini is not a controlled test of aggregation.

However, our objective is not necessarily to outperform the strongest individual LLM on every objective function.

Instead, we aim to reduce **ex-ante model-selection risk**: the most reliable LLM may vary across optimization problems and is generally unknown before solving a new black-box objective.

Rather than committing to one model based on its presumed general capability, we aggregate recommendations from multiple models to reduce dependence on any particular model.

To evaluate this motivation and address the reviewer's concern, we provide comparisons among:

1. Individual strong LLMs.
2. Seven independently sampled proposals from one strong LLM.
3. Seven proposals from heterogeneous LLMs.
4. A randomly selected proposal from the same seven-proposal pool.

We apply the same aggregation procedures, match DoE evidence and objective evaluation budgets, and report inference usage and costs separately.

We also evaluate cross-task model rankings, performance variability, and downside risk to examine whether heterogeneous aggregation provides robustness when the best individual model is unknown.

**See revised Section 4.3 [insert results and uncertainty intervals].**

We acknowledge that heterogeneous aggregation should not be claimed to offer a diversity-specific benefit unless the homogeneous-versus-heterogeneous comparison supports that conclusion.

### W2. Why use an LLM when the DoE statistics are already interpretable?

We appreciate this important question.

DoE provides interpretable statistical evidence, including main effects, interactions, and candidate rankings. These statistics can indeed be used directly for rule-based region selection.

Our motivation for involving LLMs is to investigate whether they can jointly interpret different forms of statistical evidence and variable descriptions to generate useful search-region recommendations, particularly when the decision space contains heterogeneous variables.

However, we agree that this motivation alone does not establish the necessity or added value of LLM interpretation.

We therefore include a **statistical-only region-selection baseline** that receives the same DoE summary and is followed by the same GP-BO procedure, without LLM-generated recommendations.

This comparison directly examines the incremental contribution of LLM-based interpretation beyond conventional statistical evidence.

**See revised Section 4.4 [insert results].**

We also clarify that our framework does not require LLM interpretation to outperform statistical heuristics in every setting.

### W3. What is new beyond combining established components?

We agree that combining established methods is not sufficient to demonstrate novelty without identifying the additional contribution.

Our intended contribution is not the individual introduction of DoE, LLM reasoning, or GP-based BO.

Rather, we investigate a specific mechanism:

1. Use objective-grounded statistical evidence to inform LLM-generated search proposals.
2. Aggregate recommendations to reduce reliance on an individual LLM with unknown task-specific reliability.
3. Transition toward GP-guided optimization as actual objective observations accumulate.

To evaluate the contribution of each component, we compare:

1. DoE initialization followed by conventional BO.
2. DoE with statistical-only region selection.
3. DoE with guidance from a single LLM.
4. DoE with aggregated LLM guidance.
5. The complete framework with time-varying LLM and GP guidance.

These comparisons use matched objective evaluation budgets and distinguish initialization quality from end-to-end optimization performance.

**See revised Section 4.4 [insert component-ablation results].**

We have also refined the novelty statement to emphasize the specific integration and empirical evaluation of evidence-grounded aggregation under uncertain individual-model reliability, rather than claiming novelty for the established components themselves.

### Closing remarks

We sincerely thank the reviewer again for these valuable suggestions.

We believe the additional controlled comparisons and clearer statement of our contribution will help distinguish the effects of DoE initialization, LLM interpretation, aggregation, and model diversity.

Our goal is to make the benefits and limitations of the framework directly testable, rather than relying on comparisons with a single weaker baseline.

---

# Response to Reviewer wr5s

We sincerely thank the reviewer for the careful reading and constructive comments on experimental fairness, Rastrigin initialization, theoretical interpretation, and presentation.

We appreciate the recognition of our motivation for evidence-grounded LLM aggregation and the potential value of improving robustness through collective reasoning.

We address the concerns individually below.

### W1. Is heterogeneous multi-LLM aggregation justified by the baseline?

We agree that comparing seven heterogeneous LLMs with one GPT-4o Mini does not isolate the effects of aggregation, individual model capability, number of proposals, or inference cost.

We would nevertheless like to clarify our motivation.

**Our goal is not to guarantee superiority over the best individual LLM, but to reduce dependence on a single model when its reliability for a new objective function is unknown ex ante.**

Different LLMs may perform well on different problems, and identifying the best model beforehand may itself be costly.

To investigate this motivation, we provide comparisons with strong individual LLMs, seven independent samples from one strong model using the same aggregation method, seven heterogeneous models, and random proposal selection from the same proposal pool.

We match DoE information and objective evaluation budgets, while reporting inference usage separately.

We additionally examine whether individual-model rankings vary across benchmarks and whether aggregation improves consistency and downside performance.

**See revised Section 4.3 [insert findings and uncertainty intervals].**

### W2. Does the Rastrigin design use knowledge of the global optimum?

We sincerely thank the reviewer for identifying this important concern.

We agree that information about the known global optimum should not influence the initialization of an algorithm intended for black-box optimization.

We have therefore examined the initialization procedure to distinguish the general design-generation rule from any benchmark-specific explanation provided after the experiment.

We additionally evaluate Rastrigin using a benchmark-independent, prespecified design rule that does not use knowledge of the global optimum.

The same initialization procedure is applied consistently across the compared methods.

**See revised Section 4.6 [insert verified audit conclusion and re-run results].**

We have also clarified the experimental description to ensure that it does not imply that the algorithm has access to the location of the global optimum.

### Q1. Empty Appendix A.6

We apologize for this editorial oversight.

We have removed the unfinished heading or completed the missing material, as appropriate.

**See the revised appendix.**

### Q2. Incorrect Figure 3(a) reference

Thank you for identifying this error.

We have corrected the figure-panel reference and checked the consistency of other figure references and captions.

**See revised Figure 3.**

### Q3. Inconsistent notation

We agree that consistent notation is important for interpreting both the algorithm and its theoretical analysis.

We have standardized the notation for localized search regions, the decay exponent, and related quantities throughout the main text and appendix.

We have also checked consistency between the definitions, algorithm, propositions, and proofs.

**See revised Sections 2–3 and Appendix B.**

### Q4. Do the aggregation bounds imply BO robustness or regret guarantees?

We appreciate this important theoretical question.

We agree that the original presentation did not sufficiently distinguish between theoretical properties of the aggregated proposal and guarantees for the complete Bayesian optimization process.

Our propositions characterize the behavior of aggregated recommendations under specific assumptions. They do not, by themselves, establish global convergence or end-to-end regret guarantees for the BO algorithm.

For example, suppose that $f$ is $L$-Lipschitz continuous and that the aggregated candidate $x_{\mathrm{agg}}$ satisfies

$$
\|x_{\mathrm{agg}}-x^*\|\leq\epsilon,
$$

where $x^*$ is a global minimizer.

Then,

$$
f(x_{\mathrm{agg}})-f(x^*)
\leq L\epsilon.
$$

However, this implication requires that the distance bound concerns an actual optimizer in an appropriate decision-space metric, and that the Lipschitz assumption holds.

A concentration bound around an arbitrary reference point, or a bound in an abstract representation space, does not automatically imply a corresponding objective-value guarantee.

We have therefore clarified the assumptions and theoretical scope of our aggregation propositions.

In particular, we distinguish **proposal-level robustness** from **end-to-end optimization performance**, and avoid presenting these results as complete BO regret guarantees.

The practical effects of aggregation on optimization are evaluated separately through controlled experiments.

**See revised Section 3 and Appendix B for the theoretical clarification, and Sections 4.3–4.4 for the empirical evaluation [insert results].**

### Closing remarks

We sincerely thank the reviewer again for the specific and constructive suggestions.

We believe the revised experimental comparisons, corrected initialization protocol, and clearer theoretical interpretation will strengthen the manuscript.

In particular, the revisions aim to distinguish the robustness of aggregated LLM recommendations from the performance of the complete BO procedure, while addressing the uncertainty associated with selecting an individual LLM before optimization.

---

# Final Notes Before Submission

The following placeholders must be replaced with verified content:

- **Section 4.3:** Matched-budget individual LLM, homogeneous ensemble, heterogeneous ensemble, random-selection, and direct-LLM comparisons.
- **Section 4.4:** Statistical-only DoE baseline and component ablation results.
- **Section 4.5:** DoE allocation and decay-parameter sensitivity experiments.
- **Section 4.6:** Rastrigin initialization audit and mixed-variable robustness results.
- **Sections 2–3:** Corrected notation, methodological explanations, and theoretical assumptions.
- **Appendices:** Completed proofs, definitions, and corrected references.

All claims that experiments or manuscript revisions have been completed must be checked against the actual revised paper before submission.
