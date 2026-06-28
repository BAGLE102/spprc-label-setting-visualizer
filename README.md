# SPPRC / SPPTW Label-setting Algorithm Visualizer

An interactive web-based visualization for understanding the **label-setting algorithm** used in the **Shortest Path Problem with Resource Constraints (SPPRC)** and its important special case, the **Shortest Path Problem with Time Windows (SPPTW)**.

Website:

https://bagle102.github.io/spprc-label-setting-visualizer/

## Overview

Classical shortest path algorithms usually keep only one best distance value for each node.  
However, in SPPRC, a path is evaluated not only by cost, but also by one or more constrained resources, such as time, capacity, energy, or buffer usage.

Because of these additional constraints, a node may need to keep multiple partial paths, called **labels**.  
For example, one path may arrive earlier but cost more, while another path may arrive later but cost less. Both can still be useful until the algorithm proves that one is dominated by another.

This visualizer demonstrates this idea step by step. It also provides a small graph editor, so users can change the source node, target node, node count, time windows, and edge resources directly in the browser.

## What This Visualizer Shows

The example graph is a simplified SPPTW instance. Each edge has:

- `time`: the travel or transition time
- `cost`: the objective value accumulated along the path

Each node has:

- a time window `[a, b]`
- a feasibility rule that requires arrival time to be no later than `b`

The algorithm maintains two label sets:

- `U`: unprocessed labels
- `P`: processed labels

At each step, the algorithm selects a label from `U`, extends it through outgoing edges, checks feasibility, applies dominance rules, and updates the label sets.

The graph editor supports:

- selecting a different source node
- selecting a different target node
- rebuilding the graph with a different number of nodes
- adding or removing intermediate nodes
- automatically generating directed edges
- manually editing node time windows
- manually editing edge time and cost
- adding or removing edges

## Core Concepts

### Label

A label represents one partial path from the source node to the current node.

In this visualizer, each label stores:

- current node
- accumulated time
- accumulated cost
- path history

Example:

```text
L3@C (T=8, C=7)
```

This means label `L3` is currently at node `C`, with accumulated time `8` and cost `7`.

### Resource Extension Function

When a label is extended through an edge, its resources are updated.

For this SPPTW example:

```text
raw arrival = current time + edge time
time' = max(node opening time, raw arrival)
cost' = current cost + edge cost
```

The `max()` operation represents waiting until the node's time window opens.

### Feasibility Check

After extension, the new label must satisfy the target node's time window.

```text
feasible iff time' ≤ upper bound of the node time window
```

If the new arrival time is too late, the label is discarded.

### Dominance Rule

A label can be removed if another label at the same node is no worse in every resource and strictly better in at least one resource.

In this visualizer, label `A` dominates label `B` if:

```text
A.node = B.node
A.time ≤ B.time
A.cost ≤ B.cost
and at least one inequality is strict
```

Dominance pruning is important because it prevents the number of labels from growing unnecessarily.

## Why It Matters

SPPRC is a useful modeling framework for routing problems where a path must satisfy more than one condition.

Examples include:

- vehicle routing with time windows
- constrained network routing
- energy-aware routing
- satellite or space network routing
- Delay Tolerant Networking (DTN) with contact capacity constraints

For Space DTN research, the same label-setting idea can be extended by replacing the simple `(time, cost)` resources with DTN-related resources such as:

- contact start and end time
- residual contact capacity
- bundle size
- buffer availability
- earliest arrival time
- non-fragmented bundle feasibility

## How to Use

Open the website and use the control buttons:

- `Next Step`: execute the algorithm one step at a time
- `Run All`: run the algorithm until completion
- `Reset`: restart the example

The graph highlights the current label extension, infeasible labels, dominated labels, and the final selected feasible path.

## Editing the Graph

The graph can be modified directly on the webpage.

Common operations:

- choose a different `Source` and `Target`
- change `Node count` and rebuild the graph
- edit each node's time window `[a, b]`
- edit each edge's `time` and `cost`
- add or remove edges
- press `Apply & Reset` to restart the algorithm using the updated graph

The internal graph data is still stored in `index.html`, so the page can run as a simple static website without any backend.

Each node has a time window:

```javascript
A: {x: 260, y: 90, window: [2, 8]}
```

Each edge has a time and cost:

```javascript
{from: "s", to: "A", time: 2, cost: 6}
```

## Reference

This visualizer was inspired by the explanation of label-setting algorithms for SPPRC / SPPTW in:

- Adrian Haarbach, *Shortest Paths with Resource Constraints - Label-Setting Algorithm*
