# Problem 1
# Equivalent Resistance Using Graph Theory

## Motivation

Calculating equivalent resistance is fundamental in electrical circuits for understanding and design. Traditional series and parallel simplifications become cumbersome for complex networks. Graph theory provides a systematic, algorithmic way to analyze and simplify circuits by representing them as graphs, enabling automatic handling of complex resistor configurations.

---

## Task Description

Implement an algorithm that:

- Accepts a circuit represented as a graph (nodes = junctions, edges = resistors with resistance weights).
- Iteratively simplifies the graph using series and parallel resistor reductions.
- Handles nested series and parallel connections.
- Outputs the final equivalent resistance between two terminal nodes.

---

## Algorithm Overview

1. **Represent the circuit as a graph:**
    - Nodes = junction points
    - Edges = resistors, weighted by resistance value.

2. **Iterative reduction:**
    - **Parallel reduction:** Multiple edges between the same two nodes are combined into a single edge with equivalent resistance.
    - **Series reduction:** Nodes connected to exactly two neighbors (not terminals) are removed by replacing the two series resistors with one resistor equal to their sum.

3. **Repeat reductions until only two nodes remain connected by a single edge.**

---

## Pseudocode

```text
function equivalent_resistance(graph, node1, node2):
    while graph has more than 2 nodes:
        # Reduce parallel edges
        for each pair of nodes connected by multiple edges:
            combine all parallel resistors into one resistor
        
        # Reduce series nodes
        for each node except node1 and node2:
            if node degree == 2:
                neighbors = [n1, n2]
                Req = resistance(node-n1) + resistance(node-n2)
                remove node and its edges
                add edge between n1 and n2 with Req
                
        if no reduction occurred:
            break
    
    return resistance of edge between node1 and node2
```
## Python Code
```Python
import networkx as nx

def combine_parallel_resistors(graph):
    """Reduce parallel resistors between same two nodes."""
    to_remove = []
    to_add = []
    seen = set()

    for u, v in list(graph.edges()):
        edge_key = tuple(sorted((u, v)))
        if edge_key in seen:
            continue
        seen.add(edge_key)

        data = graph.get_edge_data(u, v)
        if len(data) > 1:
            total_inv_r = sum(1 / d['resistance'] for d in data.values())
            Req = 1 / total_inv_r

            for key in list(data.keys()):
                if graph.has_edge(u, v, key=key):
                    to_remove.append((u, v, key))
            to_add.append((u, v, Req))

    for u, v, key in to_remove:
        if graph.has_edge(u, v, key=key):
            graph.remove_edge(u, v, key=key)

    for u, v, Req in to_add:
        graph.add_edge(u, v, resistance=Req)


def combine_series_resistors(graph, terminals):
    """Reduce series resistors by collapsing degree-2 non-terminal nodes."""
    for node in list(graph.nodes):
        if node in terminals or graph.degree(node) != 2:
            continue

        neighbors = list(graph.neighbors(node))
        if len(neighbors) != 2:
            continue

        data1 = graph.get_edge_data(node, neighbors[0])
        data2 = graph.get_edge_data(node, neighbors[1])
        if not data1 or not data2:
            continue

        R1 = list(data1.values())[0]['resistance']
        R2 = list(data2.values())[0]['resistance']
        Req = R1 + R2

        graph.remove_node(node)
        graph.add_edge(neighbors[0], neighbors[1], resistance=Req)
        return True
    return False


def equivalent_resistance(graph, node1, node2):
    """Compute equivalent resistance between node1 and node2."""
    terminals = {node1, node2}
    graph = graph.copy()  # Work on a copy

    while True:
        combine_parallel_resistors(graph)
        changed = combine_series_resistors(graph, terminals)
        if not changed:
            break

    combine_parallel_resistors(graph)
    if graph.has_edge(node1, node2):
        data = graph.get_edge_data(node1, node2)
        return list(data.values())[0]['resistance']
    return float('inf')


# ------------------------
# 🧪 TEST CASES
# ------------------------

def test_circuits():
    def print_result(name, G, A, B):
        Req = equivalent_resistance(G, A, B)
        print(f"{name}: Equivalent resistance between {A} and {B} = {Req:.4f} ohms")

    # 1. Simple Series: A-B-C (2Ω + 3Ω)
    G1 = nx.MultiGraph()
    G1.add_edge('A', 'B', resistance=2)
    G1.add_edge('B', 'C', resistance=3)
    print_result("Simple Series", G1, 'A', 'C')

    # 2. Simple Parallel: A-B (2Ω || 3Ω)
    G2 = nx.MultiGraph()
    G2.add_edge('A', 'B', resistance=2)
    G2.add_edge('A', 'B', resistance=3)
    print_result("Simple Parallel", G2, 'A', 'B')

    # 3. Nested (Series + Parallel): A-B-C and A-D-C, then B-C and D-C in parallel
    G3 = nx.MultiGraph()
    G3.add_edge('A', 'B', resistance=1)
    G3.add_edge('B', 'C', resistance=2)
    G3.add_edge('A', 'D', resistance=1)
    G3.add_edge('D', 'C', resistance=2)
    print_result("Nested Parallel-Series", G3, 'A', 'C')

    # 4. Bridge (Wheatstone) network with redundant paths
    G4 = nx.MultiGraph()
    G4.add_edge('A', 'B', resistance=1)
    G4.add_edge('B', 'C', resistance=1)
    G4.add_edge('C', 'D', resistance=1)
    G4.add_edge('A', 'D', resistance=1)
    G4.add_edge('B', 'D', resistance=1)
    print_result("Wheatstone Bridge", G4, 'A', 'C')

    # 5. Complex looped network
    G5 = nx.MultiGraph()
    G5.add_edge('A', 'B', resistance=2)
    G5.add_edge('B', 'C', resistance=2)
    G5.add_edge('C', 'D', resistance=2)
    G5.add_edge('D', 'A', resistance=2)
    G5.add_edge('A', 'C', resistance=1)
    G5.add_edge('B', 'D', resistance=1)
    print_result("Complex Looped", G5, 'A', 'C')

if __name__ == "__main__":
    test_circuits()
```
**The algorithm:**

    Cannot collapse series at first due to high node degrees.

    Combines parallel edges if any (not here).

    Then terminates when no series/parallel pattern is found.

✅ Works as long as a solution path exists (edge A–C exists).
    arallel Resistor Reduction:

        For each node pair, combine all parallel edges.

        At most O(E) operations per iteration, where E is the number of edges.

    Series Resistor Reduction:

        For each node (excluding terminals), check if its degree is exactly 2.

        Series reduction requires checking node neighbors and combining two edges.

        Up to O(N) per iteration, where N is the number of nodes.

    Iterations:

        The process repeats until no further reductions are possible.

        In the worst case, it can iterate O(N) times as the graph reduces.

Overall Complexity:

O(R × (N + E)),
where R is the number of reduction rounds.

    In practice, R is usually small for well-structured circuits, so the algorithm performs efficiently on most inputs.
