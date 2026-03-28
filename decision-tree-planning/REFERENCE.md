---
name: decision-tree-planning-reference
description: Reference on the decision tree as a planning and thinking mental model — branching, pruning, expected value, issue trees, sequential resolution, and practical heuristics for software delivery planning conversations.
type: reference
---

# Decision Tree as a Planning Mental Model

A decision tree is a structured thinking tool that maps a problem or decision space as a hierarchy of branching nodes. Each node represents a question, decision point, or uncertainty. Each branch represents a possible answer, path, or outcome. The tree terminates at leaf nodes — conclusions, actions, or recommendations.

This document covers the decision tree as a **cognitive and planning tool**, not a machine-learning algorithm. The two share notation but serve entirely different purposes. Here the focus is on using tree structure to reason clearly, interrogate a problem systematically, and guide sequential resolution in planning conversations.

---

## 1. Core Concept

A decision tree externalises the branching structure of a problem. Problems that feel overwhelming in prose — "what should we do about X?" — become tractable when drawn as a tree, because the tree forces:

- **Exhaustiveness**: every branch at a node must cover all possibilities (or explicitly mark "other")
- **Exclusivity**: branches should not overlap — a given situation belongs to at most one branch
- **Locality**: you only need to resolve the node you're currently at, not the whole tree at once

The tree is a model, not a map. You build it to make the reasoning explicit and auditable, not to capture every nuance of reality.

### Anatomy

```
Root node (the problem or decision)
├── Branch A (condition / choice / answer)
│   ├── Sub-node A1
│   │   ├── Leaf: outcome / action
│   │   └── Leaf: outcome / action
│   └── Sub-node A2
│       └── ...
└── Branch B (condition / choice / answer)
    └── ...
```

- **Decision node** — a choice the actor controls (square in formal notation)
- **Chance node** — an uncertain event not controlled by the actor (circle in formal notation)
- **Leaf node** — a terminal outcome, action, or recommendation
- **Branch** — the edge connecting a node to its child, labelled with the condition or choice it represents

---

## 2. Key Mechanics

### Branching

Each node asks exactly one question. Branches represent the mutually exclusive answers to that question. Good branching questions are:

- Binary when the reality is genuinely binary ("does this dependency exist?")
- Multi-way when there are discrete categories ("which team owns this?")
- Never open-ended blobs ("what are all the considerations?") — those belong as sub-trees, not single branches

**MECE discipline** (Mutually Exclusive, Collectively Exhaustive): branches at any node must not overlap, and their union must cover the full possibility space. Violations produce reasoning gaps (missing cases) or double-counting (same case traversed twice).

### Sequential Resolution

You work one node at a time, starting at the root. You do not need to resolve the whole tree before acting on a subtree. The pattern:

1. Ask the question at the current node
2. Receive an answer (from the user, from data, from exploration)
3. Follow the relevant branch — prune all other branches
4. Repeat at the next node

This is the **one-branch-at-a-time** principle. It keeps conversations focused, prevents analysis paralysis, and ensures each question is asked at the right time (not before its parent question is resolved).

### Pruning

Pruning eliminates branches that are:

- **Infeasible**: violate a hard constraint already established
- **Dominated**: another branch achieves at least as much on every dimension
- **Below threshold (satisficing)**: good enough has already been found; exhaustive search adds no value
- **Out of scope**: valid in general but not relevant to this specific problem instance

Prune eagerly. An unpruned tree grows exponentially. A well-pruned tree is a straight-line argument.

---

## 3. Expected Value Trees

When outcomes carry probability and cost/value, branches can be annotated with numbers to support quantitative comparison.

### Components

- **Probability** (p): the likelihood of a chance branch occurring, 0–1, summing to 1.0 across all branches of a chance node
- **Value / Cost** (v): the payoff or cost at a leaf node
- **Expected Value (EV)**: `EV = p × v`, rolled back up the tree from leaves to root

### Rollback Algorithm

1. Assign probabilities to all chance branches and values to all leaf nodes
2. At each chance node, compute `EV = Σ(p_i × v_i)` across children
3. At each decision node, pick the branch with the highest EV (or lowest cost, depending on framing)
4. Roll the winning EV up to the parent node
5. Repeat until the root node has an EV — this is the expected value of the optimal strategy

### Risk Adjustment

Raw EV is risk-neutral. Real decision-makers are often risk-averse. Adjustments:

- **Expected Utility (EU)**: replace monetary value with a utility function that penalises variance. A loss of £100k is not offset by a symmetric 50% chance of gaining £100k for a risk-averse organisation.
- **Worst-case / minimax**: rather than maximising EV, maximise the minimum outcome. Used in adversarial or catastrophic-risk contexts.
- **Satisficing threshold**: if a branch exceeds the "good enough" bar with high probability, take it without exhaustive comparison — reduces decision cost.

### Practical Use in Planning

In software delivery, quantitative EV trees are rarely built with precise numbers. Instead, the *structure* of EV thinking is applied qualitatively:

- "If we choose approach A, what are the likely failure modes and their probability?"
- "Which approach has the highest expected value of information — i.e., which spike or prototype would most reduce uncertainty?"
- "What is the cost of being wrong in each branch?"

---

## 4. Where Decision Trees Are Used

### Product Planning

Breaking a product decision into: user segment → problem type → solution space → build/buy/partner. Each branch is a narrowing of scope. Pruning happens when customer discovery eliminates a segment or a technical spike kills a solution path.

### Game Theory

Extensive-form games are decision trees where nodes alternate between players. Each player, at their node, chooses a branch to maximise their payoff given beliefs about future play. Backward induction (rollback from terminal nodes) is the canonical solution method — structurally identical to the EV rollback algorithm.

### Military Planning (OODA Loops)

The OODA loop (Observe, Orient, Decide, Act) is a sequential decision cycle. The "Decide" phase is often structured as a tree: given the current orientation, what are the available courses of action? Wargaming extends this by building trees for both friendly and adversary decisions, looking for branches where the adversary can cause catastrophic outcomes, then pruning plans that expose those vulnerabilities.

Commander's Intent replaces exhaustive specification: rather than pre-resolving every branch, it defines the root-level goal so that subordinates can prune and decide locally when circumstances deviate from plan.

### Consulting Frameworks

McKinsey, BCG, and Bain use **issue trees** (also called logic trees or hypothesis trees) as the primary structuring tool for problem-solving:

- The root is the client's key question ("Why is profitability declining?")
- Each level breaks the question into sub-questions
- MECE discipline is enforced at every level
- Branches are assigned to workstreams; findings are rolled back up

The issue tree is a diagnostic tree. A decision tree adds a prescriptive layer: once the diagnosis branch is resolved ("the problem is in pricing"), a decision subtree follows ("should we raise prices, reduce discounts, or change the product mix?").

### Software Delivery

- Scope decisions: feature A or B first? → depends on dependency graph and risk profile
- Architecture choices: monolith vs. services → branches narrow based on team size, operational capability, data isolation requirements
- Build vs. buy: branches on make/buy/partner driven by strategic importance × build cost × time-to-market
- Risk triage: which unknowns need a spike? → branches on uncertainty × impact

---

## 5. Issue Trees vs. Decision Trees

These terms are related but distinct. The distinction matters in planning conversations.

| Dimension | Issue Tree | Decision Tree |
|---|---|---|
| **Purpose** | Diagnose / decompose a problem | Choose between alternatives |
| **Root node** | A question or problem statement | A decision to be made |
| **Branches represent** | Sub-questions or hypotheses | Choices or chance events |
| **Leaf nodes** | Findings / root causes | Outcomes / payoffs |
| **Orientation** | Diagnostic (what is true?) | Prescriptive (what should we do?) |
| **MECE required?** | Yes — for exhaustive diagnosis | Yes — to avoid missing options |

**Hypothesis tree**: a variant of the issue tree where branches are stated as testable hypotheses ("profitability is down because variable costs increased") rather than open questions. Prioritising hypotheses for testing is itself a decision tree problem — which hypothesis, if true, would most change our recommendation? Test that one first.

**In practice**: a planning conversation moves through both types. First an issue tree to understand the problem space, then a decision tree to choose a path. The two trees share structure; the transition is the moment you shift from "what is going on?" to "what should we do about it?"

---

## 6. Decision Trees in Software Delivery Planning

Software delivery planning is rich with decision points that benefit from tree structure.

### Scoping Decisions

```
Should we build feature X now?
├── Is there a blocking dependency? (yes → resolve dependency first)
└── No blocking dependency
    ├── Is there a paying customer waiting? (yes → build now)
    └── No immediate customer pressure
        ├── Does it unlock a downstream feature we need? (yes → build now)
        └── No downstream unlock → defer
```

### Dependency Management

Dependencies are edges in a dependency graph but decisions about them are best resolved as a tree. The ordering question — "what do we build first?" — is answered by finding the branch with the longest downstream tail (most things blocked by it) and putting it first. This is equivalent to finding the critical path in a directed acyclic graph.

### Vertical Slices and the Delivery Tree

A vertical slice delivery plan is a decision tree traversal strategy:

- The root is "what is the thinnest end-to-end path?"
- Each level asks "what is the next highest-value thin slice?"
- Pruning removes slices that are dominated by alternatives or have no immediate customer value
- Leaf nodes are independently shippable increments

The anti-pattern — horizontal slicing by layer — is a failure to branch correctly. "All database work first" is not a valid branch; it answers no decision question and produces no leaf-level outcome.

---

## 7. One-Branch-at-a-Time Resolution (Socratic/Sequential Pattern)

This is the most important operational pattern for planning conversations with a human.

**The principle**: never present multiple open questions simultaneously. Resolve one branch fully before opening the next.

### Why it works

- Human working memory is limited. Two open questions compete for attention.
- Answers to early questions change what later questions are even worth asking.
- A resolved branch can be pruned; an unresolved branch must be held open mentally.
- Sequential resolution creates a clear audit trail: "we decided X because you said Y."

### The pattern

1. Identify the highest-leverage open node in the current tree
2. Ask one question that resolves that node
3. Listen to the answer; follow the branch it selects
4. Prune the other branches
5. Identify the next open node — repeat

"Highest-leverage" means: the question whose answer most changes the rest of the tree. Start with questions that prune large subtrees. Defer questions that only affect leaf details.

### Anti-patterns

- **Shotgun questioning**: listing 8 open questions at once. Forces the human to context-switch repeatedly and obscures which question is most important.
- **Premature branching**: asking about a sub-node before its parent is resolved. The answer may be invalidated when the parent is later resolved differently.
- **Re-opening pruned branches**: circling back to already-resolved nodes without a clear reason. Erodes trust in the process.

---

## 8. Pruning Strategies

Pruning is the core efficiency mechanism of the decision tree. Without it the tree explodes in size.

### Dominance Pruning

Branch B dominates Branch A if B is at least as good as A on every dimension that matters. Prune A immediately. No trade-off analysis needed.

Example: "Option B is faster, cheaper, and lower risk than Option A in every scenario we care about." Do not analyse Option A further.

### Constraint Pruning

A hard constraint established at a parent node eliminates any child branch that violates it.

Example: "We cannot use a third-party data processor due to GDPR residency requirements." Prune all branches involving external data processors downstream, immediately, without further analysis.

### Satisficing Pruning

Once a branch is found that meets all acceptance criteria ("good enough"), stop expanding alternatives. The incremental value of finding the optimal solution does not justify the cost of further search.

Example: "Option A is fast enough, cheap enough, and meets security requirements." Do not analyse Options B through F unless A fails a later gate.

### Information-Value Pruning

If resolving a branch requires information that is expensive to gather (a spike, a proof-of-concept, a legal review), prune it tentatively and revisit only if a cheaper branch fails. This is the "cheapest experiment first" principle.

### Scope Pruning (Explicit deferral)

Some branches are valid but out of scope for the current decision. Mark them explicitly as "deferred" rather than discarding — they may become relevant if the current direction fails.

---

## 9. Related Mental Models

### Second-Order Thinking

First-order thinking: "what happens if I take this branch?" Second-order thinking: "what happens as a consequence of that consequence?" In decision tree terms, second-order thinking means expanding leaf nodes one more level before committing. A branch that looks good at depth-1 may reveal a catastrophic outcome at depth-2.

Practical application: before committing to a leaf, ask "and then what?" once. If the depth-2 outcome is worse than the depth-1 outcome of a rejected branch, reopen the comparison.

### Inversion

Rather than asking "what path leads to success?", ask "what path leads to failure?" Build a failure tree from the root down. Then prune from your main tree any branches that share significant structure with the failure tree.

In software delivery: "what would cause this feature to be a disaster?" maps directly to risk branches. Build the risk tree first; it reveals which branches to avoid or instrument with reversibility.

### Counterfactual Reasoning

After committing to a branch: "what would have to be true for the rejected branch to have been correct?" This surfaces the assumptions embedded in the pruning decision and creates the conditions for early detection if those assumptions are wrong.

In planning: "we chose approach A over B because we assumed X. If X turns out to be false, we need to revisit." Counterfactuals become monitoring conditions.

### Pre-Mortem

A structured inversion technique: assume a branch has already failed and work backwards to find plausible causes. Equivalent to building a backwards tree from the failure leaf to find the fork points where different choices could have prevented it. Apply before committing, not after.

### Reversibility Weighting

When two branches have similar EV, prefer the one that is more reversible. Reversibility is an option value — if the chosen branch fails, you can return to the fork and try the other. Irreversible branches foreclose this option. This is the "two-way door vs. one-way door" distinction (common in Amazon's planning vocabulary).

---

## 10. Practical Heuristics

Rules of thumb for applying decision-tree thinking in planning conversations.

### Building the tree

- **Start with the root question, not the branches.** "What decision are we actually trying to make?" Often the stated question is a branch of a deeper question.
- **One dimension per level.** A node that branches on both "who" and "what" simultaneously is a smell — split into two levels.
- **MECE check at every branch point.** For each node: are all branches mutually exclusive? Do they collectively exhaust the possibility space? If not, add an "other" branch or refine the question.
- **Name branches with conditions, not options.** "If the user is authenticated" is better than "authenticated users" — it keeps the conditional logic explicit.

### Resolving the tree

- **Ask the highest-leverage question first.** The question that prunes the most branches is most valuable. Often this is a constraint ("can we deploy to the cloud?") not a preference.
- **Prune before you elaborate.** Before expanding a subtree, confirm its parent branch is still live. Elaborating a pruned branch is pure waste.
- **Resolve uncertainty before resolving preference.** Factual questions ("does this API support rate limiting?") should precede preference questions ("do we prefer rate limiting at the gateway or at the service?"). The answer to the first may make the second moot.
- **Timeboxed spikes to resolve chance nodes.** If a branch is blocked on an unknown probability ("we don't know if the third-party API can handle our volume"), define the smallest possible experiment that resolves it. Set a timebox. Do not continue planning the subtree until the experiment completes.

### Communicating the tree

- **Summarise the tree shape before going deep.** "There are three main branches to this decision. I'm going to resolve them left to right. We're currently on branch 2." This keeps the human oriented.
- **Name every pruned branch explicitly.** "We're not exploring option X because of constraint Y." Unnamed pruning looks like an oversight; named pruning builds trust.
- **Show the rollback path when reversing.** If new information reopens a pruned branch: "Earlier we pruned option X based on assumption Y. That assumption has changed. We need to revisit." Signal the re-entry point clearly.

### In software delivery specifically

- **Block on dependencies before elaborating dependent branches.** A branch that requires an unresolved upstream decision cannot be fully specified. Mark it "blocked on [upstream]" and park it.
- **Prefer branches with evidence over branches with assumptions.** A team that has shipped a similar thing before provides evidence; a team that has not is an assumption. Weight accordingly.
- **The critical path is the highest-value fully-resolved branch.** If the critical path is not fully resolved (no clear leaf nodes), stop and resolve it before planning anything else.
- **Reversible architecture decisions relax the tree.** If a technical decision is reversible (e.g., adding a feature flag, wrapping a third-party library behind an interface), the branch can be committed to with lower confidence. Irreversible decisions (migrating a data model, signing a vendor contract) require more tree resolution before committing.
