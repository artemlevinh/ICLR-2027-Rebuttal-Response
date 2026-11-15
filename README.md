# Response to Reviewer 5UqE

We sincerely thank the reviewer for the careful and detailed assessment. We appreciate the positive comments and thoughtful suggestions, which have helped us improve the clarity of the manuscript. We address each concern below.

**W1.** We agree that Figure 1 contained too many implementation details. We have simplified it to show the main workflow: collect DoE evidence, obtain evidence-grounded LLM proposals, aggregate promising regions, and conduct sequential GP-based optimization. The detailed steps are explained in the text and appendix. **See revised Figure 1 and Section 2.1.**

**W2.** We thank the reviewer for highlighting the unclear notation and methodological explanations. We have clarified the definitions of factor types, DoE summaries, region representations, and search operations, and simplified the pseudocode.

We would also like to respectfully clarify that Theorem 1 provides a probability bound showing that **when most LLMs propose regions close to a target region, their aggregated medoid is also likely to remain close to that region**. This supports our motivation for collective reasoning: aggregation can reduce the influence of a minority of unreliable proposals. We agree, however, that the intuition and assumptions behind this result were not sufficiently explained. We have expanded the discussion to clarify how the guarantee depends on the proportion of reliable proposals and the independence assumption, while distinguishing proposal-level robustness from end-to-end BO performance.

**See revised Sections 2–3 and Appendix B.**

**W3.** We agree that the DoE fraction trades off initial statistical evidence against evaluations available for sequential BO. Our choice of $\rho_{\mathrm{DoE}}=0.3$ is empirically motivated by comparisons of different allocations under a fixed total evaluation budget. We now report the corresponding sensitivity analysis and explain the selection rule. We appreciate the suggestion of adaptive allocation and regard it as a useful extension. **See revised Section 4.5.**

**W4.** We apologize for the ambiguity. The quantity $L_{i,s}$ in the trust-region expression denotes the scalar size of the region around observed anchor $x_i$ at iteration $s$, as previously listed in the notation appendix. We have moved this definition closer to the equation and specified the numerical-coordinate normalization $\phi_j$ used in the region construction. **See revised Section 2.4 and the notation appendix.**

**W5.** We appreciate this important question. Our scheduling rationale is motivated by prior studies, including LLAMBO (Liu et al., 2024), LLINBO (Chang et al., 2025), and AuxBO (Gupta et al., 2026), which highlight the usefulness of LLM guidance when observations are limited. In particular, we draw inspiration from the decaying LLM-influence strategies in LLINBO and AuxBO, which gradually shift optimization toward data-driven BO as more observations become available. **Our distinctive contribution is to ground LLM recommendations in statistical evidence obtained through DoE**, enabling evidence-informed identification of promising subregions rather than relying primarily on pretrained LLM knowledge.

Regarding parameter selection, $\rho_0=0.10$ maintains a fixed probability of global random exploration, while $\lambda_0=0.80$ controls the initial balance between LLM-guided and GP-guided search. Both are empirically chosen. In contrast, the choice of $\gamma=4$ (or larger) is motivated by our theoretical analysis, with its justification and supporting verification provided in the appendix.

**See revised Section 2.4 and the Appendix.**

**W6.** We thank the reviewer for this important question. We primarily use GPT-4o Mini as a baseline to maintain comparability with previous LLM-assisted BO studies, such as LLAMBO (Liu et al., 2024) and LLINBO (Chang et al., 2025), which used earlier GPT-family models. Our objective is not to demonstrate superiority over a relatively weaker LLM, but to show that evidence-grounded collective reasoning can provide benefits even when compared with earlier-generation models.

Importantly, a stronger LLM does not necessarily guarantee better optimization performance. Model reliability may vary across objective functions, and the best-performing model cannot be reliably identified beforehand. Our approach therefore aggregates recommendations from multiple LLMs to reduce dependence on any single model. We have further examined this question through additional experiments reported in the revised Appendix.

Nevertheless, we agree that stronger-model comparisons are important. We have added experiments comparing individual strong LLMs, seven proposals from one strong model, seven heterogeneous LLMs, and random or strong-LLM selection from the same proposal pool, with matched objective evaluation budgets and inference costs reported separately. **See revised Section 4.3.**

Regarding the distance metric, average normalized distance measures how close the proposed initial points are to the known global optimum, averaged over experimental runs. It evaluates initialization quality rather than final optimization performance, which we separately assess using simple regret. **See revised Section 4.1.**

**W7.** We thank the reviewer for these observations. Regarding Rastrigin in Figure 3(a), LLAMBO's strong early performance may be explained by the function's well-known structure, particularly its global optimum at $(0,0)$. An LLM familiar with this benchmark may repeatedly propose points near the origin without learning the landscape from observations. Similar behavior is illustrated in **Figure 2(f), page 6, of LLINBO (Chang et al., 2025)**.

Regarding Bukin in Figure 3(e), we agree that LLAMBO's performance is somewhat surprising. This may reflect the limited evaluation budget of 50 and the function's narrow, nonsmooth valley, which makes its landscape difficult to characterize. Moreover, with only $N_{\mathrm{MC}}=20$ runs, favorable stochastic outcomes may contribute to the observed performance. We have clarified these possible explanations in **Section 3**.

Regarding the suggested direct-LLM comparison, we appreciate this recommendation. Our existing FCNet ablation in Section 3, Figure (b), already examines the separate contributions of DoE evidence and LLM guidance. We have further expanded this analysis with additional results in Figures (c) and (d), providing stronger evidence that the performance gains are largely attributable to DoE-grounded reasoning rather than LLM guidance alone.

**See revised Section 3 and Figures (b)–(d).**

**W8.** We respectfully clarify that our experiments are not limited to continuous domains. FCNet Naval includes nine categorical, integer-valued, and numerical hyperparameters, and our x264 application optimizes over constrained discrete software configurations. We agree that the evidence can be broadened further, and have added another heterogeneous-variable benchmark to examine the generality of the approach. **See revised Section 4.6.**

**W9.** The initially slower improvement reflects the evaluations allocated to DoE: these observations characterize the objective landscape rather than immediately exploiting a small set of promising candidates. This evidence helps identify promising subregions for subsequent optimization, explaining the later catch-up in the regret curves. We have added a visualization of the learned landscape and selected regions to illustrate this mechanism. **See the revised Appendix.**

**W10.** We apologize for the unclear terminology. The *LLM warm-start GP* baseline uses LLM-proposed points for its initial objective evaluations, then proceeds with standard GP-based BO. We have made this definition explicit in the revised manuscript.

**W11.** We thank the reviewer for this useful baseline. We compare the same DoE initialization followed by conventional pattern search, without LLM proposals, under an identical total evaluation budget. We also organize the broader component analysis around four settings—neither DoE nor LLM guidance, DoE only, LLM guidance only, and both—to distinguish their individual and joint contributions. **See revised Section 4.4.**

**W12.** We agree that the main text should emphasize the reasoning behind the method rather than implementation detail. We have simplified the figure and pseudocode, defined notation consistently, expanded the intuitive explanations, and moved secondary algorithms and technical details to the appendix. **See revised Sections 2–3 and the Appendix.**

We sincerely thank the reviewer again for the detailed feedback. We believe these revisions better communicate the specific roles of objective-grounded evidence, collective proposal aggregation, and sequential optimization, while making their individual contributions easier to evaluate.

---

# Response to Reviewer 5LFU

We sincerely thank the reviewer for the constructive assessment and for recognizing the motivation and empirical promise of the framework. We appreciate the request for stronger component identification and have clarified the corresponding experimental comparisons.

**W1.** We appreciate the concern that the original comparison with one GPT-4o Mini may reflect differences in model capability or proposal count as well as aggregation. Our motivation is to obtain reliable guidance when it is not known beforehand which model will be most effective for a new objective. Aggregation can reduce dependence on a single potentially misleading proposal, but we agree that its value should be tested against strong alternatives. We therefore compare individual strong models, seven proposals sampled from the same strong model, seven heterogeneous proposals, and random selection from an identical proposal pool. We additionally test whether a strong LLM selecting one proposal from that pool is preferable to medoid or projected-mean aggregation. These comparisons maintain the same DoE evidence and objective evaluation budgets, with inference costs reported separately. We evaluate both average optimization quality and robustness across tasks. **See revised Section 4.3.**

**W2.** We agree that DoE statistics such as estimated effects, interactions, and ranked candidates can themselves guide region selection. Our reason for using LLMs is to combine these measured patterns with factor descriptions and feasibility constraints when recommending promising subregions; this does not imply that an LLM is always necessary. To assess its incremental contribution, we compare an LLM-free statistical region-selection rule with LLM-guided selection using the same DoE observations and downstream GP-BO procedure. We also examine DoE-only and LLM-only variants as part of the component study. **See revised Section 4.4.**

**W3.** We thank the reviewer for this important question. We agree that DoE, LLM reasoning, and BO are individually established methods. Our contribution lies in **grounding LLM reasoning in statistical evidence obtained from DoE**, allowing LLMs to identify promising subregions based on observed objective behavior rather than relying primarily on pretrained knowledge, which may be insufficient or biased in practical black-box optimization tasks.

Our existing FCNet ablation in Section 3, Figure (b), provides evidence of the contributions of these components. The full method and the No in-loop LLM variant follow identical trajectories during the initial DoE stage, confirming that their early performance comes from the shared experimental design rather than subsequent LLM guidance. After initialization, the full method continues improving and eventually outperforms the No in-loop LLM variant, suggesting that continued LLM-guided search provides additional benefits. Meanwhile, the No DoE/no evidence variant remains competitive, indicating that DoE grounding is beneficial but not uniformly superior at every evaluation budget.

We have further expanded the analysis with Figures (c) and (d) to better isolate the contributions of DoE evidence and LLM reasoning.

**See revised Section 3 and Figures (b)–(d).**

---

# Response to Reviewer wr5s

We sincerely thank the reviewer for the careful reading and thoughtful comments on the experiments, presentation, and theoretical results. We appreciate the recognition of the motivation for evidence-grounded collective reasoning and address the concerns below.

**W1.** We appreciate the concern that comparing seven heterogeneous LLMs with one GPT-4o Mini does not by itself determine which aspect of the ensemble generates the gains. Our motivation is that individual LLM reliability may vary across tasks, so relying on one model selected beforehand can be fragile. Aggregation is intended to reduce this dependence, not to assume that averaging must outperform every strong alternative. We have expanded the comparisons to strong individual models, seven independent proposals from one strong model, heterogeneous seven-model aggregation, and random selection from the same proposal pool. We also compare aggregation with a strong LLM selecting among those same proposals. Matching DoE evidence and objective evaluation budgets, while reporting inference cost separately, allows us to assess the roles of proposal count, model capability, and aggregation. **See revised Section 4.3.**

**W2.** We thank the reviewer for identifying this issue. We agree that a black-box optimizer should not use knowledge of the global minimizer when choosing its initial design. The original explanation of Rastrigin's design referred to the optimum at the origin and was therefore inappropriate as a justification for initialization. We have adopted a benchmark-independent design rule and repeated the Rastrigin comparison under that rule, without using optimum information to select the DoE points. **See revised Section 4.6.**

**Q1.** We apologize for the confusion. Appendix A.6 was actually included in the original submission, but a LaTeX formatting issue caused the section heading and corresponding figure to appear in different locations, giving the impression that the section was empty. We have corrected the formatting and verified the placement of the section and figures in the revised appendix.
**Q2.** Thank you for catching the incorrect Figure 3(a) reference. We have corrected the panel attribution and checked the other figure labels and captions. **See revised Figure 3.**

**Q3.** We agree that inconsistent notation makes the method harder to follow. We have standardized the notation for the local search domain and decay exponent throughout the main text, algorithms, and appendix, and have simplified the corresponding pseudocode. **See revised Sections 2–3 and Appendix B.**

**Q4.** We appreciate this important distinction. The medoid result bounds the distance of the aggregated region from a reference region when a sufficient majority of proposals are accurate. Its probability statement additionally assumes independent proximity events; using different LLMs alone does not establish independence. The projected-mean result likewise controls representation-space error, not objective regret directly. These are useful proposal-level robustness statements, but we agree that they do not automatically give an end-to-end BO guarantee. For example, an objective-value bound follows if the aggregate is close to a *global optimizer* in a suitable decision-space metric and the objective is Lipschitz; without those conditions, a distance bound is insufficient. We have made these assumptions and limitations explicit and present the end-to-end performance separately through the controlled experiments. **See revised Section 3, Appendix B, and Sections 4.3–4.4.**

We thank the reviewer again. We believe the revised comparisons and more precise theoretical discussion improve the interpretation and reproducibility of the framework.
