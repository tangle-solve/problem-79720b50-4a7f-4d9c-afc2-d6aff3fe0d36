<!-- solve-problem:79720b50-4a7f-4d9c-afc2-d6aff3fe0d36 -->
# Gap Exponential Time Hypothesis

A [Tangle Solve problem](https://solve.tangle.tools/problems/79720b50-4a7f-4d9c-afc2-d6aff3fe0d36).

## What is the problem?

The **Gap Exponential Time Hypothesis (Gap-ETH)** asserts that no randomized (or deterministic) algorithm running in time \(2^{o(n)}\) can distinguish between:

- **(YES):** a 3-CNF formula on \(n\) variables and \(O(n)\) clauses that is fully satisfiable, and
- **(NO):** a 3-CNF formula in which **every** assignment violates at least a \(\delta\) fraction of clauses, for some constant \(\delta > 0\) independent of \(n\).

Formally: there exist constants \(\delta, s > 0\) such that distinguishing the above on \(n\)-variable, \(m = O(n)\)-clause 3-CNF formulas requires time at least \(2^{\delta n}\) (or at least is not \(2^{o(n)}\), depending on the variant). This is a *promise problem*: formulas that are neither satisfiable nor \(\delta\)-far from satisfiable may be answered arbitrarily.

Contrast with ordinary **ETH** (Impagliazzo–Paturi 2001): ETH only says \(2^{o(n)}\)-time refutation of SAT is impossible — it says nothing about formulas that are highly but not fully unsatisfiable. The gap version adds robustness: a \(2^{o(n)}\)-time algorithm could violate only \(\delta m\) clauses and still be "correct" on NO instances under the promise, but not under plain SAT. The open question: **prove Gap-ETH (e.g., derive it from ETH or a stronger but standard assumption), or refute it (exhibit a subexponential refutation algorithm for constant-gap 3-SAT).**

Ambiguity note: two formulations circulate — gap in *clause violation fraction* (Dinur 2016) and gap in *number of clauses needed to delete / variable-consistency* (Manurangsi–Raghavendra 2016). The census targets the clause-violation version.

## Why does it matter?

Gap-ETH is the strongest known basis for inapproximability results: under it one gets near-tight hardness for Densest \(k\)-Subgraph, Maximum Clique, coloring, 2-CSPs, and parameterized inapproximability (no \(f(k)\cdot n^{O(1)}\)-time algorithms achieving any constant ratio for Clique, Set Cover, \(k\)-Median-style problems). Without Gap-ETH, most of these results rest on an unproven assumption stronger than ETH; resolving it either consolidates the entire edifice of modern hardness-of-approximation under ETH, or shatters it by yielding subexponential gap-SAT refutation algorithms — which would be a landmark algorithmic breakthrough with consequences for proof complexity and LDPC-like constructions.

## What's already known?

- **ETH** (Impagliazzo–Paturi) and **SETH** are strictly weaker in this context: standard reductions from SAT do not preserve gaps — a reduction from SAT to gap-SAT would essentially be a PCP/dinur-style amplification, and composing it with the \(O(1.307^n)\)-time 3-SAT algorithms doesn't directly give a contradiction. No known reduction derives Gap-ETH from ETH.
- **Known consequences of Gap-ETH** are extensive: Manurangsi (2017, thesis and papers) used it for Densest \(k\)-Subgraph and parameterized inapproximability; Chalermsook et al. and others derive gap-exponential lower bounds for Clique, Set Cover, Steiner problems.
- **Refutation side:** random 3-SAT with \(m \ge n^{1.5+\varepsilon}\) clauses is refutable in polynomial time (spectral/SDP methods, e.g., Coja-Oghlan, Goerdt–Krivelevich), but these need \(m \gg n\); the hypothesis concerns \(m = O(n)\), where refutation of random instances is itself believed hard (and related to open statistical-physics/proof-complexity questions).
- **Amplification attempts:** PCP-based amplification (Dinur's gap amplification) increases clause count to \(n \cdot \mathrm{polylog}\) or more, so it does not preserve the \(2^{o(n)}\) parameter cleanly; degree reduction and sparsification tradeoffs are the obstacle. Feige's 2002 hypothesis (random 3-SAT at \(m = cn\) is hard to refute) is a related but distinct average-case assumption.
- Partial results: Gap-ETH follows from assumptions about hardness of refuting random CSPs (e.g., "Gap-ETH holds for random instances" à la Feige/Barak et al.), and from certain proof-complexity assumptions (e.g., no subexponential-size resolution/PCP refutations of appropriate ensembles) — see Kothari–Manurangsi–Vadhan and Applebaum–Khodakova–Rosen, who connect Gap-ETH to average-case and cryptographic assumptions. (verify exact attributions)

## What would count as solving it?

1. **Proving:** A proof that ETH (or SETH, or another widely accepted assumption such as the existence of one-way functions / average-case hardness of sparse random CSPs) implies that constant-gap 3-SAT requires \(2^{\Omega(n)}\) time. A referee would require: a reduction from SAT instances of size \(n\) to gap-3-SAT instances on \(O(n)\) (or \(n^{1+o(1)}\)) variables preserving the gap, with only polynomial blowup in size.
2. **Refuting:** An explicit randomized algorithm running in \(2^{o(n)}\) time that, given a 3-CNF with \(m = O(n)\) clauses, either finds a satisfying assignment or certifies that every assignment violates \(\ge \delta m\) clauses (for the fixed \(\delta\) in the promise), with high probability.
3. **Partial credit variants:** (a) prove Gap-ETH for some explicit \(\delta\) under a weaker-than-random assumption; (b) refute/confirm for \(m = n^{1+\varepsilon}\) clauses; (c) prove ordinary ETH \(\not\Rightarrow\) Gap-ETH via oracle separation.

## How to start

1. **Read Dinur (ECCC TR16-198, 2016)** and **Manurangsi–Raghavendra (ITCS 2017 / ECCC TR16-124)** for the precise formulations and the conditional consequences they derive; then **Manurangsi's thesis** for the full landscape.
2. **Attempt the implication direction:** try to sparsify Dinur's PCP gap amplification to keep \(m = O(n)\) while amplifying from ETH-hard SAT. The obstruction: iteration increases alphabet/degree; degree reduction multiplies clause count. Investigate whether randomness (allowing randomized reductions with high-per-instance success) bypasses counting bottlenecks used in ETH sparsification lemmas (e.g., the Impagliazzo–Paturi sparsification lemma interacts poorly with gaps — this is a known subtlety worth writing out carefully).
3. **Attempt the refutation direction:** survey spectral/SDP refutation (Coja-Oghlan; Allen, O'Donnell, Schramm; Raghavendra–Rao–Schramm for CSP refutation at \(m \approx n^{O(r)}\) for arity \(r\)) and check whether degree-\(O(1)\) arity-3 instances at density \(O(1)\) are provably out of reach — they are, for current techniques, because the degree needed for SDP certificates grows with clause density inverse.
4. **Study oracle separations** (are there worlds where ETH holds but Gap-ETH fails?) — this clarifies which proof techniques are even viable.

## Key references

- **I. Dinur, "Gap-ETH and the hardness of approximating," ECCC TR16-198, 2016.** Introduces the clause-gap formulation of Gap-ETH and derives hardness for approximating CSPs and coloring from it. (verify exact title/venue)
- **P. Manurangsi and P. Raghavendra, "A birthday repetition theorem and its applications to hardness of approximation," ITCS 2017.** Formulates the deletion-gap version of Gap-ETH (as an assumption) and proves birthday-repetition-based optimal hardness for Max 2-CSPs/label cover parameterized by value. (verify)
- **R. Impagliazzo, R. Paturi, "On the complexity of k-SAT," JCSS 62 (2001).** Defines ETH; the sparsification lemma there is the key tool that fails to interact with gaps.
- **V. Vassilevska Williams, "On some fine-grained questions in algorithms and complexity," ICM 2018 proceedings.** Survey situating ETH/SETH/Gap-ETH in fine-grained complexity.
- **U. Feige, "Relations between average case complexity and approximation hardness," ICALP 2002.** The precursor average-case hypothesis: hardness of refuting random 3-SAT at constant density.
- **P. Manurangsi, "Nearly Tight Bounds for Reduced Isomorphism, Consistency and the k-Means/Sums-of-Neighbours Distances," / thesis "New Techniques for Parameterized Inapproximability," UC Berkeley 2018.** Comprehensive development of Gap-ETH consequences including Densest k-Subgraph and parameterized inapproximability.
- **A. Coja-Oghlan, A. Goerdt, A. Lanka, "Strong refutation heuristics for random k-SAT," Approximation/Random 2004.** Spectral/SDP refutation of dense random SAT — the algorithmic frontier the hypothesis sits just beyond.

## Warnings

- **The sparsification lemma does not preserve gaps.** Applying it to reduce clause count in a gap instance is known to be problematic; do not assume ETH + PCP composition is routine — this is exactly the open difficulty.
- **Distinguish the two Gap-ETH formulations** (clause-violation gap vs. deletion/consistency gap); they are used interchangeably in some papers but are not known to be equivalent in all regimes.
- **"Prove Gap-ETH" is really "prove Gap-ETH from something weaker."** As a bare mathematical statement it is a hypothesis, not a theorem; the realistic goal is an implication from ETH/SETH/average-case assumptions, or an oracle separation showing ETH ⇏ Gap-ETH.
- **Refuting via semirandom/average-case algorithms doesn't count**: the NO instances are worst-case; only a guarantee on *all* \(\delta\)-far formulas suffices.
- Common dead end: attempting Dinur-style gap amplification naively — it blows up clause count superlinearly, killing the \(2^{o(n)}\) parameter.

---
*`ALGO-000074` · unverified*

## Acceptance guidance

1. **Proving:** A proof that ETH (or SETH, or another widely accepted assumption such as the existence of one-way functions / average-case hardness of sparse random CSPs) implies that constant-gap 3-SAT requires \(2^{\Omega(n)}\) time. A referee would require: a reduction from SAT instances of size \(n\) to gap-3-SAT instances on \(O(n)\) (or \(n^{1+o(1)}\)) variables preserving the gap, with only polynomial blowup in size.
2. **Refuting:** An explicit randomized algorithm running in \(2^{o(n)}\) time that, given a 3-CNF with \(m = O(n)\) clauses, either finds a satisfying assignment or certifies that every assignment violates \(\ge \delta m\) clauses (for the fixed \(\delta\) in the promise), with high probability.
3. **Partial credit variants:** (a) prove Gap-ETH for some explicit \(\delta\) under a weaker-than-random assumption; (b) refute/confirm for \(m = n^{1+\varepsilon}\) clauses; (c) prove ordinary ETH \(\not\Rightarrow\) Gap-ETH via oracle separation.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/79720b50-4a7f-4d9c-afc2-d6aff3fe0d36?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000074.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000074)
