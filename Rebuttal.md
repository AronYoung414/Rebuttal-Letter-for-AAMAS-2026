# Rebuttal Letter for AAMAS 2026

## Reviewer 7xAW:
We sincerely thank the reviewer for the careful reading of our paper and the
positive assessment of both the theoretical contributions and the empirical
evaluation. We thank the reviewer for recognizing the thoroughness of our
empirical study. We will further strengthen the presentation in the revised manuscript.

## Reviewer arPd:
We thank the reviewer for the thoughtful and detailed feedback, and for recognizing the relevance, clarity, and organization of our work. We address all raised concerns below and will incorporate the suggested improvements in the revised manuscript.

* [TI-OI-Dec-POMDPs] We will clarify that our work studies observation-independent Dec-POMDPs in the introduction and preliminary. The reviewers are correct that section 3.2 and 3.3 (“Inferring Environment State Sequence” and “Estimating Environment Secrets”) assume both transition-independence and observation-independence. We will highlight the assumptions in the introduction.  Section 3.1 (“Inferring Latent State Sequence”) does not require transition independence, and only assumes only conditional independence of observations (Assumption 1).  We will clarify this distinction in the revision.

* In section 3.2 and 3.3, due to both transition and observation independence, the dec-POMDP model with a reduced agent set can be easily constructed. We will clarify this definition in the revision.  In section 3.1, we do not need to redefine a transition model for a reduced agent set because
when selecting a subset of agents (K), the *other* agents still act according to their policies and participate in the transition dynamics, but  their observations are excluded from the inference objective. For example, suppose the system has 5 agents and a subset (K={1,2,4}) of agents are used for active joint state estimation, all 5 agents execute their respective policies and evolve under the joint transition model, while only the observation histories of agents 1,2,4 are used for the perception objective, that is, computing the joint-state trajectory estimation  using the agents 1,2,4's observations. 
We will clarify this more explicitly in Section 3.

* [Revised Proof of Lemma 3] We appreciate the reviewer pointing out the issue in the proof of Lemma 3. Indeed, while $H(X_e | Z, Y_A)$ is **supermodular**, equation (6) shows that
  $I(Z;Y_A) = I(X_e;Y_A) - H(X_e) + H(Z) + H(X_e|Z,Y_A)$ so establishing submodularity in $I(Z;Y_A)$ requires further assumption.

We provide the corrected lemma and proof sketch as follows: In A TI-OI-Dec-POMDP, for any $Y_A$ and $Y_B$, $Y_A\subseteq Y_B$, if
    $H(X_e| Y_A \cup \{Y_j\}) - H(X_e|Z, Y_A \cup \{Y_j\})    \leq H(X_e| Y_B \cup \{Y_j\})- H(X_e|Z, Y_B \cup \{Y_j\})$, then $I(Z;Y_A)$ is submodular.

**Revised proof** We aim to show that $I(Z;Y_A)$ is submodular. From the equation (6), $I(Z;Y_A) = I(X_e;Y_A) - H(X_e) + H(Z) + H(X_e|Z,Y_A)$, we only need to prove that $I(X_e;Y_A) + H(X_e|Z,Y_A)$ is submodular since $-H(X_e) + H(Z)$ is a constant. Let  $\Omega = \{Y_i, i \in \mathcal{N}\}$ be the set of observations for all agents. $Y_A\subset Y_B\subset \Omega$, and $Y_j \in \Omega\setminus Y_B$. If $I(X_e;Y_A) + H(X_e|Z,Y_A)$ is submodular, then it satisfies 

$I(X_e; Y_A \cup \{Y_j\}) +H(X_e|Z, Y_A \cup \{Y_j\}) - I(X_e; Y_A ) -  H(X_e|Z,Y_A) \geq I(X_e; Y_B \cup \{Y_j\}) +H(X_e|Z, Y_B \cup \{Y_j\}) - I(X_e; Y_B  ) -  H(X_e|Z,Y_B)  $ (Diminishing return property).

Rearranging the terms, XXXXX

This condition intuitively states that the entropy reduction gained from knowing the secret Z should not decrease as more information is collected. The proof is as follows. 

Expand the mutual information, we have
$H(X_e| Y_A \cup \{Y_j\}) - H(X_e| Y_B \cup \{Y_j\}) \leq H(X_e|Z, Y_A \cup \{Y_j\}) - H(X_e|Z, Y_B \cup \{Y_j\})$

By moving the terms, we obtain the   condition $$- H(X_e|Z, Y_A \cup \{Y_j\}) + H(X_e| Y_A \cup \{Y_j\}) \leq - H(X_e|Z, Y_B \cup \{Y_j\}) + H(X_e| Y_B \cup \{Y_j\})$$.

    * In cases when the condition does not hold, equation (6) shows that our objective $I(Z;Y_A)$ is a **monotone (submodular + supermodular) function**. Hence, results from Bai & Bilmes (2018) [4] provide curvature-dependent approximation guarantees for greedymax selection.   Approximate-submodularity results [5] also offer similar performance guarantees. We plan to investigate these directions for the future extension.

* We thank the reviewer for raising this point.
In our grid-world model, a sensor returns the *exact* robot location whenever detection occurs (under zero observation noise). Thus, increasing the sensing range increases the probability of detection and therefore always increases information gain. We will clarify this modeling assumption and note that saturation effects may occur in settings where observations are noisy or ambiguous.

* We will expand the baseline discussion.
The key reason for using IPG is that our objective is **mutual information**, not a cumulative reward or value function. As a result:
  * Value-function-based MARL algorithms (MADDPG, MAPPO) are not applicable.
  * Dec-POMDP solvers require either a reward structure or a belief-based value function.
  * IPG is the only decentralized gradient-based method that directly optimizes a policy without relying on a value function.
We will add a clearer explanation of both IPG’s mechanics and why it is the most appropriate available baseline.
* We thank the reviewer for all detailed editorial suggestions. We will correct all grammatical issues, punctuation, hyphenation, equation punctuation, and notation inconsistencies in the camera-ready version.

## Reviewer iAnQ:

We thank the reviewer for highlighting the limitations of our empirical section and provide clarifications and commitments below.

* We appreciate the reviewer’s detailed feedback on notation clarity. We agree and will revise the paragraph to introduce other symbols for notations. We will change the S and T for the subsets of observations space. We will replace the time index symbol by t consistently throughout and avoid using T as an index.
* We will shorten the abstract presentation of submodularity and directly introduce it together with the agent-selection context in Section 3, allocating more space to key proofs.
* We thank the reviewer for highlighting the limitations of our empirical section and provide clarifications and commitments below. Our primary goal was to evaluate *information gain* under the IMAS² framework in a controlled environment. The binary type-classification task is chosen because:
  * it directly corresponds to inferring the latent discrete variable (Z) in Section 3.3,
  * the classification difficulty increases significantly with stochastic dynamics and limited sensing, and
  * the MI/entropy values (our main theoretical metric) are interpretable (within $[0, 1]$) under binary uncertainty.
Nevertheless, we agree that richer tasks (multi-class labels, multi-target tracking, or continuous latent variables) would better demonstrate generality. We will expand the discussion and include this limitation explicitly.
* We agree additional baselines strengthen empirical context.
However, algorithms in [6,7] assume reward-based or value-based objectives, or centralized belief updates, which are not compatible with our *information-theoretic* objective and decentralized constraint. Still, we can include the comparison with:
  * a random subset selector,
  * a visibility/coverage-based selector, and
  * a fixed-location MI-ranking baseline,
performance for comparison with IMAS².
* We agree statistical analysis is essential. In the revision, we can report:
  * mean ± standard deviation for entropy and accuracy,
  * per-sensor MI gain variance,
  * confidence intervals for the classification matrices.
* We will also fully specify:
  * number of runs,
  * time horizon (T),
  * rollout termination rules,
  * policy-gradient training parameters.
* We will provide all missing details:
  * hardware: Intel i7-12700 CPU, 64GB RAM (no GPU used),
  * number of gradient updates per iteration,
  * batch size (M) and horizon (T),
  * total runtime for each method, not only time per iteration.
These specifications will allow meaningful comparison against IPG and other baselines.



    

## References
[1] Raphen Becker, Shlomo Zilberstein, Victor Lesser, and Claudia V. Goldman. 2004. Solving transition independent decentralized Markov decision processes. J. Artif. Int. Res. 22, 1 (July 2004), 423–455.

[2] Ranjit Nair, Pradeep Varakantham, Milind Tambe, and Makoto Yokoo. 2005. Networked distributed POMDPs: a synergy of distributed constraint optimization and POMDPs. In Proceedings of the 19th international joint conference on Artificial intelligence (IJCAI'05). Morgan Kaufmann Publishers Inc., San Francisco, CA, USA, 1758–1760.

[3] Frans A. Oliehoek and Christopher Amato. 2016. A Concise Introduction to Decentralized POMDPs (1st. ed.). Springer Publishing Company, Incorporated.

[4] Bai, W. &amp; Bilmes, J.. (2018). Greed is Still Good: Maximizing Monotone Submodular+Supermodular (BP) Functions. <i>Proceedings of the 35th International Conference on Machine Learning</i>, in <i>Proceedings of Machine Learning Research</i> 80:304-313 Available from https://proceedings.mlr.press/v80/bai18a.html.

[5] Horel, T. and Singer, Y. Maximization of approximately submodular functions. In Advances In Neural Information Processing
Systems, pp. 3045–3053, 2016.

[6] Stefano V. Albrecht and Peter Stone. Reasoning about Hypothetical Agent Behaviours and their Parameters. In AAMAS '17, International Foundation for Autonomous Agents and Multiagent Systems, 547–555.

[7] Shafipour Yourdshahi, E., do Carmo Alves, M.A., Varma, A. et al. On-line estimators for ad-hoc task execution: learning types and parameters of teammates for effective teamwork. Auton Agent Multi-Agent Syst 36, 45 (2022).


