# AI Lab Concepts Compilation

This document compiles the major AI problem-solving techniques used across your labs and gives a structured study guide for:

- BFS / DFS
- A\*
- Hill Climbing
- Minimax
- Alpha-Beta Pruning
- Genetic Algorithm

It also includes one unified, generic model you can apply to most AI search/optimization/game problems.

## 1) Unified Generic Model (Most Important)

Almost all the above techniques fit into one template.

### 1.1 Problem as a State-Space Model

Define a problem using:

- State $s$: current situation.
- Initial state $s_0$.
- Actions $A(s)$: legal moves from $s$.
- Transition $T(s, a) \rightarrow s'$.
- Goal test $G(s)$: whether solved.
- Path cost $g(s)$ (if costs matter).
- Heuristic $h(s)$ (estimated remaining effort).
- Evaluation $f(s)$:
  - Search: often $f(s)=g(s)+h(s)$
  - Hill climbing: often $f(s)=\text{objective}(s)$
  - Games: utility/value from Minimax

### 1.2 Generic Solve Loop

1. Put initial state into frontier/open list/population.
2. Pick next candidate by strategy:
   - Queue order (BFS)
   - Stack order (DFS)
   - Best $f(s)$ (A\*)
   - Best neighbor (Hill Climbing)
   - Best game value (Minimax/Alpha-Beta)
   - Best chromosomes (GA)
3. Expand/generate successors.
4. Keep promising candidates, discard others.
5. Stop when goal/terminal/convergence criterion is met.

### 1.3 What Changes Across Techniques

- Selection rule
- Memory usage
- Guarantee (optimal/complete vs approximate)
- Need for heuristic/evaluation
- Deterministic vs stochastic behavior

## 2) BFS and DFS

### 2.1 Core Idea

- BFS explores level by level using a queue.
- DFS explores deep first using a stack/recursion.

### 2.2 Properties

- BFS:
  - Complete on finite branching.
  - Optimal for unweighted graphs (minimum edges).
  - Higher memory use.
- DFS:
  - Lower memory use.
  - Not optimal in general.
  - Can get stuck deep without careful visited handling.

### 2.3 Complexity (branching factor $b$, depth $d$, max depth $m$)

- BFS: time $O(b^d)$, space $O(b^d)$
- DFS: time $O(b^m)$, space $O(bm)$

### 2.4 Typical Exam/Lab Pattern

Given a graph and start/goal:

1. Show BFS visit order and shortest path.
2. Show DFS visit order/path found.
3. Compare path length and expansions.

## 3) A\* Search

### 3.1 Core Idea

A\* selects state with minimum:

$$
f(n)=g(n)+h(n)
$$

- $g(n)$: exact cost from start.
- $h(n)$: estimated cost to goal.

### 3.2 Heuristic Conditions

- Admissible: never overestimates.
  $$h(n) \le h^*(n)$$
- Consistent (monotone):
  $$h(n) \le c(n,n') + h(n')$$

If $h$ is admissible (graph-search with proper reopen/consistency handling), A\* is optimal.

### 3.3 Why A\* Works Better Than Greedy Best-First

- Greedy uses only $h(n)$ and can choose cheap-looking but expensive routes.
- A\* balances known cost and future estimate.

## 4) Hill Climbing

### 4.1 Core Idea

Iteratively move to a better neighbor using objective value.

- Maximization version: move to neighbor with highest value.
- Stop when no improvement exists.

### 4.2 Strengths and Weaknesses

- Fast, simple, memory-light.
- Can fail due to:
  - Local maxima
  - Plateaus
  - Ridges

### 4.3 Common Fixes

- Random-restart hill climbing
- Sideways moves (limited)
- Simulated annealing (probabilistic escape)

## 5) Minimax

### 5.1 Core Idea

For two-player, zero-sum, perfect-information games:

- MAX player tries to maximize utility.
- MIN player tries to minimize utility.

Recursive rule:

- Terminal state: return utility.
- MAX node: return max(child values).
- MIN node: return min(child values).

### 5.2 Utility Convention (example)

- $+1$ = MAX wins
- $0$ = draw
- $-1$ = MIN wins

### 5.3 Complexity

- Time $O(b^d)$
- Space $O(bd)$ (depth-first implementation)

## 6) Alpha-Beta Pruning

### 6.1 Core Idea

Same result as Minimax, fewer nodes evaluated.

Maintain:

- $\alpha$: best value MAX can guarantee so far.
- $\beta$: best value MIN can guarantee so far.

Prune when:

$$
\alpha \ge \beta
$$

### 6.2 Impact

- Best case effective branching factor greatly reduced.
- Move ordering quality strongly affects pruning amount.

### 6.3 Important Fact

Alpha-Beta does not change final optimal move from Minimax; it only avoids unnecessary evaluation.

## 7) Genetic Algorithm (GA)

### 7.1 Core Idea

Population-based stochastic optimization inspired by natural evolution.

Pipeline:

1. Initialize population.
2. Evaluate fitness.
3. Select parents.
4. Crossover to create offspring.
5. Mutate offspring.
6. Replace population.
7. Repeat until stop criterion.

### 7.2 Representation Choices

- Binary string
- Permutation (e.g., route/TSP-like)
- Real-valued vector

### 7.3 Key Hyperparameters

- Population size
- Crossover rate
- Mutation rate
- Selection pressure
- Number of generations

### 7.4 Strength and Limitation

- Strength: good for large, irregular, non-differentiable spaces.
- Limitation: no strict guarantee of global optimum.

## 8) Quick Technique Selection Guide

- Need shortest path in unweighted graph: BFS
- Need memory-efficient deep exploration: DFS
- Need optimal path with weighted cost and heuristic: A\*
- Need quick local optimization: Hill Climbing
- Need adversarial game move: Minimax
- Need faster adversarial search: Alpha-Beta
- Need global/stochastic optimization over huge space: GA

## 9) Solved Example Question Templates (Manual Style)

### Q1: BFS/DFS

Given graph and start=A, goal=G:

- BFS frontier evolution gives shortest-edge path.
- DFS gives a depth-priority path depending on neighbor order.

Expected discussion: compare expansion order, path length, and completeness/optimality.

### Q2: A\*

Given weighted graph and heuristic table:

1. Compute $f=g+h$ for frontier nodes each step.
2. Expand minimum $f$.
3. Stop at goal and report path + total cost.

Expected discussion: whether heuristic seems admissible and how expansions differ from UCS/Greedy.

### Q3: Hill Climbing

Given objective $f(x)$ and discrete neighbor relation:

1. Start at initial $x$.
2. Evaluate neighbors.
3. Move to best improving neighbor.
4. Stop when no better neighbor exists.

Expected discussion: local max vs global max.

### Q4: Minimax + Alpha-Beta

Given depth-limited game tree with leaf utilities:

1. Back up min/max values bottom-up.
2. Show final root decision.
3. For alpha-beta, show where pruning occurs using $\alpha,\beta$ updates.

Expected discussion: same final move, fewer explored leaves with pruning.

### Q5: Genetic

Given chromosome encoding and fitness function:

1. Evaluate initial population.
2. Select top parents.
3. Perform crossover and mutation.
4. Create next generation.
5. Compare best fitness before/after.

Expected discussion: exploration vs exploitation and effect of mutation rate.

## 10) Common Mistakes Checklist

- BFS/DFS:
  - Forgetting visited set (causes loops)
  - Mixing queue and stack behavior
- A\*:
  - Wrong priority key (must be $g+h$)
  - Not updating/reopening better-cost states
- Hill Climbing:
  - Assuming found optimum is global
- Minimax/Alpha-Beta:
  - Wrong terminal utility signs
  - Updating alpha/beta at wrong node type
- GA:
  - Fitness direction confusion (maximize vs minimize)
  - Mutation too low (premature convergence) or too high (random search)

## 11) Final Memory Hook

You can remember all six with one sentence:

- Search methods (BFS/DFS/A\*) pick which state to expand.
- Local optimization (Hill Climbing) picks which neighbor to move to.
- Game methods (Minimax/Alpha-Beta) pick which move survives opponent response.
- Evolutionary methods (GA) pick which population members reproduce.

If you need, this can be converted into a one-page exam revision sheet next.
