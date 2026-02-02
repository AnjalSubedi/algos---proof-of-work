# Algorithmic Reasoning Proof — Shortest Path (BFS + Dijkstra)

## Problem statement
Compute the shortest path distance (and optionally the path) from a source node `s`
to a target node `t` in a graph.

- Inputs: graph `G = (V, E)`, source `s`, target `t`
- Output: shortest distance `dist[t]` (and path reconstruction if needed)
- Constraints: handle graphs with cycles; may be disconnected

## Approaches considered
### A) BFS (unweighted graphs)
- Idea: explore nodes in layers by number of edges from `s`.
- When valid: all edges have equal weight (unweighted).
- Time: O(V + E)
- Space: O(V)
- Pros: fastest/simple; optimal for unweighted graphs
- Cons: incorrect when edge weights differ

### B) Dijkstra (non-negative weighted graphs)
- Idea: maintain best-known distances; repeatedly “finalize” the node with smallest tentative distance using a min-heap.
- When valid: all edge weights are non-negative.
- Time: O((V + E) log V) with a binary heap
- Space: O(V)
- Pros: handles weights; gives optimal shortest paths
- Cons: slower than BFS on unweighted graphs due to heap overhead

## Chosen approach
- If the graph is unweighted: use BFS for O(V + E).
- Otherwise (non-negative weights): use Dijkstra for correctness with weights.

## Correctness argument (sketch)
### BFS
Invariant: when a node `u` is first dequeued, the discovered distance `dist[u]` equals
the minimum number of edges from `s` to `u`.
Reason: BFS explores nodes in non-decreasing distance layers (0, 1, 2, ...). A shorter
path would have placed `u` in an earlier layer, contradicting first discovery.

### Dijkstra
Invariant: when a node `u` is extracted from the min-heap, `dist[u]` is final and equals
the true shortest-path distance from `s` to `u`.
Reason: assume there exists a shorter path to `u` at extraction time. That path must
reach `u` from some predecessor `p` with `dist[p]` smaller than `dist[u]` plus a non-negative
edge weight—meaning `p` would have been extracted earlier and would have relaxed `u` to a
smaller value. Contradiction. Non-negative weights are essential to prevent later paths
from becoming cheaper.

## Complexity analysis
- BFS:
  - Time: O(V + E) (each node/edge processed at most once)
  - Space: O(V) for queue + distance/visited arrays/maps
- Dijkstra (binary heap):
  - Time: O((V + E) log V) (heap ops per extract-min / relaxation)
  - Space: O(V) for dist/prev + heap storage

## Tests that demonstrate correctness thinking
- Disconnected graph: target unreachable → return sentinel (e.g., inf / None)
- Cycles: ensure algorithm terminates and returns correct shortest distance
- Multiple equal shortest paths: distance correct regardless of chosen parent
- Zero-weight edges (Dijkstra): still correct
- Random small graphs: compare Dijkstra vs a slower baseline (e.g., Bellman-Ford) for validation
