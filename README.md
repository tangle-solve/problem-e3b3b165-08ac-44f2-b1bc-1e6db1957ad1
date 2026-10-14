<!-- solve-problem:e3b3b165-08ac-44f2-b1bc-1e6db1957ad1 -->
# MAD-DAG with competing coalitions

A [Tangle Solve problem](https://solve.tangle.tools/problems/e3b3b165-08ac-44f2-b1bc-1e6db1957ad1).

## What is the problem?

MAD-DAG (arXiv:2511.21552) is a DAG-based blockchain protocol whose reward mechanism is designed so that a **single** rational miner gains nothing from deviating from honest behavior (e.g., withholding blocks or reorganizing the DAG): deviations trigger mutual destruction ("MAD" = mutually assured destruction) in which the deviator's own rewards are burned or nullified. The open problem is to extend the safety and incentive-compatibility analysis to a setting with:

- **Multiple coordinating attackers**: several miners (or mining pools), each with hashpower αᵢ, Σαᵢ < 1 (or even collectively > 1), who can form coalitions and jointly deviate.
- **Heterogeneous network delays**: block propagation delays that differ per miner pair (typically modeled with a Δ-synchrony bound or a stochastic gossip model).
- **Dynamic coalitions**: coalition membership can change over time; agreements are non-binding (no side payments enforceable off-chain, though side-payment coalitions are often modeled).

The formal object is a stochastic game: state = current DAG plus outstanding unpublished blocks per player; actions = mine on chosen tip(s), publish/withhold; payoffs = protocol rewards. The target claims are (i) **safety** (no profitable deviation exists for any coalition structure, or safety degrades gracefully and is quantified), and (ii) **incentive compatibility**: honesty is an equilibrium — ideally a strong or coalition-proof Nash equilibrium — in this game. The census leaves the exact equilibrium concept unspecified; the most reasonable formalization is ε-coalition-proof Nash equilibrium under a Δ-bounded network, which the solver should state explicitly.

## Why does it matters?

A single-attacker guarantee is not deployable: real DAG ledgers (blockdag protocols in the Nakamoto/PHANTOM/GhostDAG lineage) face multiple large pools that can and do coordinate informally. A multi-attacker, delay-aware analysis would tell protocol designers whether MAD-DAG's burning mechanism survives collusion — the same question that sank plain selfish-mining analyses when generalized to multi-attacker settings. Failure to resolve it blocks any credible deployment claim and leaves the 2025 result a curiosity rather than a security theorem.

## What's already known?

- The MAD-DAG paper (Bar-Zur, Tamar, Eyal, 2025) establishes incentive compatibility against a **single** rational miner, presumably in a simplified delay model (verify the exact synchrony assumption — this is the first thing to check). Its technique is presumably a deviator-punishment/burn argument: detectable withholding triggers reward destruction, making the deviation unprofitable.
- Perešíni et al. (2026) analyze selfish mining with multiple attackers in conventional (tree) blockchains, showing multi-attacker dynamics differ qualitatively from single-attacker ones — e.g., attackers can attack each other, and equilibrium structures fragment. This is the template for what "competing coalitions" does to MAD-DAG.
- From the general literature: coalition-proofness in stochastic games is hard; standard tools are Markov-perfect equilibrium computation, strong Nash / coalition-proof Nash refinements (Bernheim–Peleg–Whinston), and robust mechanism design. Known dead end: naive single-deviator burn mechanisms are often broken by two-player collusion (one deviates, coalition splits the loss).

## What would count as solving it?

Any of the following, in decreasing strength:

1. **Upper bound / impossibility**: exhibit a coalition of two miners with total hashpower α < (some bound) and a delay profile under which joint deviation is strictly profitable — i.e., MAD-DAG is not coalition-proof. A concrete attack strategy with a computed payoff improvement.
2. **Positive result / matching construction**: prove honesty is an ε-coalition-proof Nash equilibrium for coalitions up to total hashpower α*, in a Δ-bounded heterogeneous-delay network, and characterize the tight α*. Or modify the reward rule to restore coalition-proofness and prove the new mechanism.
3. **Characterization**: solve the stochastic game for the two-player, two-hashpower case and give exact equilibrium payoffs as functions of (α₁, α₂, delays), showing where collusion pays.

A referee wants: explicit model (delay model, tie-breaking, reward rule), explicit equilibrium concept, and a proof or a verified attack simulation.

## How to start

1. Read MAD-DAG (arXiv:2511.21552) closely to extract the exact game model and punishment mechanism; identify which step of the single-attacker proof fails when two players share information/unpublished blocks.
2. Read Perešíni et al. (arXiv:2601.02984) for the multi-attacker selfish-mining machinery (Markov chain state spaces for competing withholders) — likely reusable directly.
3. Compute the two-player case first: can miner 2 "shield" miner 1's withholding from the burn trigger? If the punishment is triggered by detectable gaps, collusion that interleaves publications may evade detection — formalize this as a candidate impossibility.
4. Main obstacle: the state space of the multi-player DAG-withholding game explodes; consider restricting to a stylized two-tip DAG abstraction before the full protocol.

## Key references

- Bar-Zur, Tamar, Eyal, "MAD-DAG," arXiv:2511.21552 (2025). Introduces the MAD-DAG protocol and proves single-rational-miner incentive compatibility for a DAG ledger with a mutual-destruction reward rule. https://arxiv.org/abs/2511.21552 (verify availability; identifier post-dates reliable knowledge).
- Perešíni et al., "Selfish Mining in Multi-Attacker Scenarios," arXiv:2601.02984 (2026). Analyzes selfish mining with multiple simultaneous attackers, showing inter-attacker competition reshapes equilibria. https://arxiv.org/abs/2601.02984 (verify; not independently checkable here).
- Background (standard, safe to rely on): Bernheim, Peleg, Whinston (1987), "Coalition-Proof Nash Equilibria I. Concepts," *JET* — the equilibrium refinement to adopt.

## Warnings

- Both arXiv identifiers come from a census record dated after my knowledge; verify they exist and match the descriptions before building on them.
- "MAD-DAG safety" conflates two notions: consensus safety (no conflicting blocks finalized) and incentive safety (no profitable deviation). The 2025 paper presumably addresses the latter for one miner; keep the two claims separate.
- Delays matter enormously: burn-based punishments relying on "missing blocks" are unsound under genuine network asynchrony — an honest slow miner looks like a withholder. Any impossibility or possibility result must pin down the delay model first.
- Common dead end: assuming side payments are impossible. If colluders can transfer funds off-chain, many burn mechanisms collapse trivially — decide upfront whether to model transfers.

---
*`BCGT-000251` · flagged for review*

## Acceptance guidance

Any of the following, in decreasing strength:

1. **Upper bound / impossibility**: exhibit a coalition of two miners with total hashpower α < (some bound) and a delay profile under which joint deviation is strictly profitable — i.e., MAD-DAG is not coalition-proof. A concrete attack strategy with a computed payoff improvement.
2. **Positive result / matching construction**: prove honesty is an ε-coalition-proof Nash equilibrium for coalitions up to total hashpower α*, in a Δ-bounded heterogeneous-delay network, and characterize the tight α*. Or modify the reward rule to restore coalition-proofness and prove the new mechanism.
3. **Characterization**: solve the stochastic game for the two-player, two-hashpower case and give exact equilibrium payoffs as functions of (α₁, α₂, delays), showing where collusion pays.

A referee wants: explicit model (delay model, tie-breaking, reward rule), explicit equilibrium concept, and a proof or a verified attack simulation.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/e3b3b165-08ac-44f2-b1bc-1e6db1957ad1?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000251.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000251)
