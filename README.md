# Response to Reviewer 5UqE

We thank the reviewer for the detailed assessment and constructive suggestions. We address each concern below.

**W1.** We have simplified Figure 1 to show DoE evidence, LLM region proposals, aggregation, and sequential BO, separating implementation details from the overview. See Figure 1 and Section 2.1.

**W2.** We have clarified notation and shortened the pseudocode. Theorem 1 states that when a majority of proposals are close to a reference region, their medoid remains within a bounded distance. Independence is required for the probability statement, not the deterministic majority bound. We now explain these assumptions and distinguish proposal robustness from BO regret guarantees. See Sections 2–3 and the theoretical appendix.

**W3.** We selected $\rho_{\mathrm{DoE}}=0.3$ through experimental comparisons under a fixed total evaluation budget. This balances initial evidence against sequential evaluations. The revised sensitivity analysis explains this choice. Our current allocation is predetermined; adapting it using BO feedback remains a useful extension. See Section 4.5.

**W4.** $L_{i,s}$ is the scalar trust-region size around anchor $x_i$ at iteration $s$. We have moved its definition from the notation appendix to the corresponding equation and clarified the numerical-coordinate transformation $\phi_j$. See revised Section 2.4 and the notation appendix.

**W5.** Our early LLM guidance follows measured DoE evidence, not an ungrounded warm start. Aggregation limits reliance on one model; the schedule shifts query selection toward GP search as observations accumulate. LLINBO and AuxBO motivate decay, while our contribution is DoE-grounded collective guidance. The empirical settings $\rho_0=0.10$ and $\lambda_0=0.80$ preserve global exploration and control the initial LLM/GP balance. We use $\gamma=4$ to control transition speed, not as a universal no-harm threshold or an inherited LLINBO guarantee. See Section 2.4 and the Appendix.

**W6.** We compare GPT-5.6 alone, seven separately sampled GPT-5.6 proposals with aggregation, seven heterogeneous proposals with the same aggregation, and uniform selection from the identical heterogeneous pool. DoE evidence and objective evaluation budgets are matched; inference costs are reported separately. Shared-pool comparisons isolate aggregation; homogeneous-versus-heterogeneous comparisons evaluate ensemble designs, not diversity alone. See Section 4.3.

Table 1 concerns initialization: “Avg. Dist.” measures normalized input-space proximity to a global optimizer; “Regret” measures an objective-value gap, not distance or final regret at budget $T$. We distinguish these from end-to-end simple regret in Section 4.1.

**W7.** For Rastrigin, repeated proposals near $(0,0)$ could explain LLAMBO's early advantage; benchmark familiarity remains a possible explanation, not an established cause. On Bukin, we acknowledge LLAMBO's better performance within 50 evaluations. Its advantage cannot be attributed to luck merely because $N_{\mathrm{MC}}=20$. See the numerical-results discussion in Section 3.

The requested direct-LLM optimizer chooses each next point from the available context and observations without our GP/aggregation stage. Our expanded FCNet ablations examine component contributions but do not, by themselves, replace that direct comparison. See Section 3.

**W8.** Our experiments are not exclusively continuous. FCNet Naval contains categorical choices, integer-valued parameters, and numerical parameters on finite grids; x264 uses constrained discrete configurations. These support applicability beyond continuous functions, without establishing effectiveness on every mixed-domain problem. See the FCNet and x264 experiments.

**W9.** DoE initially spends evaluations on landscape characterization rather than immediate exploitation. The resulting evidence supports region selection, consistent with later catch-up. Added landscape and region visualizations illustrate this mechanism without establishing it from regret curves alone. See the revised Appendix.

**W10.** The LLM warm-start GP baseline evaluates LLM-proposed initial points, then runs standard GP-based BO. We have clarified this definition.

**W11.** We include identical DoE initialization followed by pattern search without LLM guidance, with the same total evaluation budget. This is a conventional direct-search comparator, distinct from our router's named Direct-Search path. The broader component study compares DoE and LLM guidance separately and jointly. See Section 4.4.

**W12.** We have simplified the main figure and pseudocode, standardized notation, expanded the methodological explanations, and moved secondary details to the appendix. Thank you again for these helpful suggestions.

---

# Response to Reviewer 5LFU

We sincerely thank the reviewer for the constructive assessment and for recognizing the motivation and empirical promise of the framework. We appreciate the request for stronger component identification and have clarified the corresponding experimental comparisons.

**W1.** We thank the reviewer for this important observation. We appreciate that the original comparison with GPT-4o may not fully distinguish model capability, proposal count, and aggregation.

Our motivation is that the most reliable LLM for a given optimization problem cannot always be identified beforehand. To examine this motivation more rigorously, we introduce **GPT-5.6 as a strong single-model baseline** and compare it against seven independently sampled GPT-5.6 proposals with aggregation and our original seven heterogeneous LLMs with the same aggregation procedure.

We additionally compare aggregation with random selection from the same seven-proposal pool. These experiments help distinguish the contributions of model capability, repeated sampling, model diversity, and aggregation. We maintain consistent DoE evidence and objective evaluation budgets, while reporting inference costs separately.

**See revised Section 4.3.**

**W2.** We thank the reviewer for this important question. While DoE provides statistical evidence such as main effects, interactions, and candidate rankings, **translating these findings into promising and feasible search subregions remains a challenging task**. Our key contribution is to enable LLMs to jointly interpret statistical evidence, variable descriptions, and constraints, transforming observed objective behavior into actionable guidance for subsequent BO.

Importantly, our FCNet ablation already provides empirical support for the contribution of LLM guidance. In Figure (b), the full method and the No in-loop LLM variant share identical initial DoE observations, but the full method achieves better optimization performance during subsequent evaluations. This suggests that LLM guidance contributes beyond the initial statistical screening.

We have further expanded this ablation in Figures (c) and (d), including a statistical-only region-selection baseline using the same DoE evidence, to more directly evaluate the additional contribution of LLM-based interpretation.

**See revised Section 3 and Figures (b)–(d).**

**W3.** We thank the reviewer for this important question. While DoE, LLM reasoning, and BO are individually established techniques, **our key contribution lies in their principled integration to enable statistically grounded LLM reasoning for black-box optimization**, rather than simply combining existing methods.

Unlike conventional LLM-guided optimization, which may rely heavily on pretrained knowledge or general heuristics, our framework allows LLMs to interpret statistical evidence collected directly from the target system through DoE. This enables them to identify promising search subregions based on observed objective behavior. Such grounding is particularly important when pretrained knowledge is incomplete, biased, or poorly aligned with the unknown objective landscape. **Our approach therefore shifts LLMs from primarily knowledge-driven advisors toward evidence-grounded optimization agents**, supporting more informed and adaptive search-space refinement.

To examine the contribution of this mechanism, our FCNet ablation in Section 3, Figure (b), separates the effects of DoE evidence and LLM guidance. We further expand this analysis in Figures (c) and (d), providing additional evidence on the individual and combined contributions of these components.

**See revised Section 3 and Figures (b)–(d).**
---

# Response to Reviewer wr5s

We sincerely thank the reviewer for the careful reading and thoughtful comments on the experiments, presentation, and theoretical results. We appreciate the recognition of the motivation for evidence-grounded collective reasoning and address the concerns below.

**W1.** We appreciate the reviewer's concern regarding the fairness of our original multi-LLM comparison. Our objective is to provide reliable optimization guidance without assuming that the best-performing LLM for a new task is known beforehand.

To address this concern, we have added **GPT-5.6 as a stronger individual baseline**. We further compare seven independent proposals from GPT-5.6 with seven proposals from our heterogeneous LLM ensemble, applying the same aggregation method. A random-selection baseline using the identical heterogeneous proposal pool helps isolate the benefit of aggregation itself.

All comparisons use matched DoE evidence and objective evaluation budgets, with inference costs reported separately. This allows us to more clearly evaluate the respective effects of model strength, proposal diversity, and collective aggregation.

**See revised Section 4.3.**

**W2.** We thank the reviewer for raising this concern. We agree that a black-box optimizer should not exploit knowledge of the global optimum during initialization. We would like to clarify that our statement about using 8 CCD points and 5 maximin points for Rastrigin was intended to explain how we **avoided directly sampling the known optimum at $(0,0)$**, rather than using its location to guide optimization. A standard CCD may include the center point, which coincides with Rastrigin's global optimum. Evaluating this point during initialization would make the problem artificially easy and obscure the contribution of DoE-based statistical evidence and subsequent optimization.

We apologize that our original wording did not make this distinction clear. We have clarified the design rationale and initialization procedure in the revised manuscript.

**Q1.** We apologize for the confusion. Appendix A.6 was actually included in the original submission, but a LaTeX formatting issue caused the section heading and corresponding figure to appear in different locations, giving the impression that the section was empty. We have corrected the formatting and verified the placement of the section and figures in the revised appendix.

**Q2.** Thank you for catching the incorrect Figure 3(a) reference. We have corrected the panel attribution and checked the other figure labels and captions. **See revised Figure 3.**

**Q3.** We agree that inconsistent notation makes the method harder to follow. We have standardized the notation for the local search domain and decay exponent throughout the main text, algorithms, and appendix, and have simplified the corresponding pseudocode. **See revised Sections 2–3 and Appendix B.**

**Q4.** We appreciate this important distinction. The medoid result bounds the distance of the aggregated region from a reference region when a sufficient majority of proposals are accurate. Its probability statement additionally assumes independent proximity events; using different LLMs alone does not establish independence. The projected-mean result likewise controls representation-space error, not objective regret directly. These are useful proposal-level robustness statements, but we agree that they do not automatically give an end-to-end BO guarantee. For example, an objective-value bound follows if the aggregate is close to a *global optimizer* in a suitable decision-space metric and the objective is Lipschitz; without those conditions, a distance bound is insufficient. We have made these assumptions and limitations explicit and present the end-to-end performance separately through the controlled experiments. **See revised Section 3, Appendix B, and Sections 4.3–4.4.**

We thank the reviewer again. We believe the revised comparisons and more precise theoretical discussion improve the interpretation and reproducibility of the framework.
