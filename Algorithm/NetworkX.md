# NetworkX

It is for creating, editing, and analyzing complex networks such as graph.

```bash
# To install
pip install networkx[default]
```

## Simple usage

You can create predefined or customized graph and attach labels to them.

```python
import networkx as nx

# Create a custom graph
H = nx.path_graph(10)
print(H)

# Create a graph
G = nx.Graph()

# Add some nodes and edges
G.add_node(1)
G.add_node(2)
G.add_node("spam")
G.add_edge(1, 2)
G.add_edge(1, "spam")
G.add_nodes_from([
    (4, {"color": "red"}),
    (5, {"color": "green"}),
])
print(G)
print(G.edges)
print(G.nodes)

# ----- output ------
# Graph with 10 nodes and 9 edges
# Graph with 5 nodes and 2 edges
# [(1, 2), (1, 'spam')]
# [1, 2, 'spam', 4, 5]
```

## Drawing

You can use `matplotlib` beside NetworkX to draw the graphs

```python
import networkx as nx
import matplotlib.pyplot as plt

# Draw the graphs
R = nx.petersen_graph()
sub_graph_1 = plt.subplot(111)
nx.draw(R, with_labels=True, font_weight='bold')
plt.show()

# For dense graph and any case that optimization is required, 
# the package will use the scipy package to have a better performance.
M = nx.complete_graph(500)
nx.draw(M)
plt.show()
```

## Create random graph

```python
import networkx as nx

g = nx.fast_gnp_random_graph(10, 0.5)
print(g.edges)
```