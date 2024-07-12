# Formula-1-Data-Analytics-Project-

# Formula 1 Data Analytics Project

## Project Overview

The Formula 1 Data Analytics Project aims to explore and visualize the relationships between Formula 1 constructors, drivers, and circuits using network analysis techniques. By employing NetworkX, a Python library for the creation, manipulation, and study of complex networks, and Matplotlib, for visualization, this project provides insights into the structure and connectivity within the Formula 1 ecosystem.

### Objectives

- **Visualize hierarchical relationships** between constructors, drivers, and circuits.
- **Explore bipartite graphs** to understand the connections between constructors and drivers.
- **Analyze network metrics** to measure the centrality and connectivity of nodes within the network.

## Data

The project uses the following datasets:

- **Constructors Data**: Contains information about Formula 1 constructors.
- **Drivers Data**: Contains details about Formula 1 drivers.
- **Circuits Data**: Provides data about Formula 1 race circuits.

### CSV Files

- `data/constructors.csv`
- `data/drivers.csv`
- `data/circuits.csv`

## Code Implementation

### Hierarchical Network Visualization

The hierarchical network visualization represents the relationships between constructors, drivers, and circuits. The nodes are categorized into three types: constructors, drivers, and circuits, and are connected to represent their interactions.

```python
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Load the CSV files
constructors = pd.read_csv('data/constructors.csv')
drivers = pd.read_csv('data/drivers.csv')
circuits = pd.read_csv('data/circuits.csv')

# Sample data preparation
top_constructors = constructors.head(10)  # Select top 10 constructors
top_drivers = drivers.head(20)            # Select top 20 drivers
top_circuits = circuits.head(10)          # Select top 10 circuits

# Create a NetworkX graph
G = nx.DiGraph()

# Add nodes with attributes
for index, row in top_constructors.iterrows():
    G.add_node(row['name'], type='constructor', label=row['name'])
    
for index, row in top_drivers.iterrows():
    G.add_node(row['driverRef'], type='driver', label=row['driverRef'])
    
for index, row in top_circuits.iterrows():
    G.add_node(row['name'], type='circuit', label=row['name'])

# Define relationships (for demonstration purposes)
for constructor in top_constructors['name']:
    for driver in top_drivers['driverRef']:
        G.add_edge(constructor, driver, relation='employs')

for driver in top_drivers['driverRef']:
    for circuit in top_circuits['name']:
        G.add_edge(driver, circuit, relation='raced_at')

# Define node colors and sizes based on type
color_map = {
    'constructor': '#1f77b4',  # Blue for constructors
    'driver': '#ff7f0e',       # Orange for drivers
    'circuit': '#2ca02c'       # Green for circuits
}

node_colors = [color_map[G.nodes[node]['type']] for node in G.nodes]
node_sizes = {
    'constructor': 700,
    'driver': 500,
    'circuit': 400
}
sizes = [node_sizes[G.nodes[node]['type']] for node in G.nodes]

# Define positions using a hierarchical layout
pos = nx.multipartite_layout(G, subset_key='type')

# Draw the nodes and edges
plt.figure(figsize=(16, 12))

# Draw nodes
nx.draw_networkx_nodes(G, pos, node_color=node_colors, node_size=sizes, edgecolors='k', linewidths=1.5)

# Draw edges
nx.draw_networkx_edges(G, pos, arrowstyle='-|>', arrowsize=20, edge_color='gray', alpha=0.6)

# Draw labels
nx.draw_networkx_labels(G, pos, labels={n: G.nodes[n]['label'] for n in G.nodes}, font_size=12, font_weight='bold')

# Add title
plt.title('Hierarchical Network of Constructors, Drivers, and Circuits in Formula 1', fontsize=18, fontweight='bold')

# Add a legend
handles = [
    plt.Line2D([0], [0], marker='o', color='w', markerfacecolor=color_map['constructor'], markersize=15, label='Constructors'),
    plt.Line2D([0], [0], marker='o', color='w', markerfacecolor=color_map['driver'], markersize=15, label='Drivers'),
    plt.Line2D([0], [0], marker='o', color='w', markerfacecolor=color_map['circuit'], markersize=15, label='Circuits')
]
plt.legend(handles=handles, loc='upper left')

# Manually adjust layout to ensure everything fits well
plt.subplots_adjust(left=0.05, right=0.95, top=0.95, bottom=0.05)

# Display the plot
plt.show()

```
![download](https://github.com/user-attachments/assets/b548ab2e-6d30-41ad-84f6-2d9204b746ba)
## **Bipartite Graph Visualization**
- The bipartite graph shows the relationship between constructors and drivers. Each node belongs to one of two sets: constructors or drivers. This type of graph is useful for understanding which drivers are associated with which constructors.
```python
- import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Load the CSV files
constructors = pd.read_csv('data/constructors.csv')
drivers = pd.read_csv('data/drivers.csv')

# Sample data preparation
top_constructors = constructors.head(10)  # Select top 10 constructors
top_drivers = drivers.head(20)            # Select top 20 drivers

# Create a Bipartite Graph
B = nx.Graph()

# Define the sets of nodes
constructors_set = top_constructors['name'].tolist()
drivers_set = top_drivers['driverRef'].tolist()

# Add nodes with bipartite set attributes
B.add_nodes_from(constructors_set, bipartite=0)  # Set 0
B.add_nodes_from(drivers_set, bipartite=1)       # Set 1

# Add edges between the two sets (for demonstration purposes, randomly connect)
for constructor in constructors_set:
    for driver in drivers_set:
        B.add_edge(constructor, driver)

# Define positions for the bipartite layout
pos = nx.bipartite_layout(B, nodes=constructors_set)

# Draw the bipartite graph
plt.figure(figsize=(14, 10))

# Draw nodes
nx.draw_networkx_nodes(B, pos, nodelist=constructors_set, node_color='lightblue', node_size=500, label='Constructors')
nx.draw_networkx_nodes(B, pos, nodelist=drivers_set, node_color='lightgreen', node_size=500, label='Drivers')

# Draw edges
nx.draw_networkx_edges(B, pos)

# Draw labels
nx.draw_networkx_labels(B, pos, font_size=8, font_weight='normal')

# Add a legend
import matplotlib.lines as mlines
constructors_legend = mlines.Line2D([], [], color='lightblue', marker='o', linestyle='None', markersize=10, label='Constructors')
drivers_legend = mlines.Line2D([], [], color='lightgreen', marker='o', linestyle='None', markersize=10, label='Drivers')
plt.legend(handles=[constructors_legend, drivers_legend], loc='upper left', fontsize=10)

# Add a multiline title
plt.title('Bipartite Graph of Constructors and\nDrivers', fontsize=14, fontweight='normal')

# Adjust layout to fit everything nicely
plt.subplots_adjust(left=0.05, right=0.95, top=0.9, bottom=0.05)

# Display the plot
plt.show()
```
![download](https://github.com/user-attachments/assets/cb5a062b-b9fb-4b4a-bd94-c343e3bbc704)


## **Updated Bipartite Graph with Metrics**
-This section adds new nodes and edges to the bipartite graph, and calculates centrality metrics to analyze the importance of nodes within the network.
```python
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Load the CSV files
constructors = pd.read_csv('data/constructors.csv')
drivers = pd.read_csv('data/drivers.csv')

# Sample data preparation
top_constructors = constructors.head(10)  # Select top 10 constructors
top_drivers = drivers.head(20)            # Select top 20 drivers

# Create a Bipartite Graph
B = nx.Graph()

# Define the sets of nodes
constructors_set = top_constructors['name'].tolist()
drivers_set = top_drivers['driverRef'].tolist()

# Add nodes with bipartite set attributes
B.add_nodes_from(constructors_set, bipartite=0)  # Set 0 for constructors
B.add_nodes_from(drivers_set, bipartite=1)       # Set 1 for drivers

# Add edges between the two sets
for constructor in constructors_set:
    for driver in drivers_set:
        B.add_edge(constructor, driver)

# Example of adding a new driver
new_driver = 'New Driver'
drivers_set.append(new_driver)
B.add_node(new_driver, bipartite=1)
# Add an edge between the new driver and an existing constructor
B.add_edge(new_driver, constructors_set[0])

# Example of removing a node safely
node_to_remove = constructors_set[0]  # Replace with a valid node name from your dataset

if node_to_remove in B.nodes:
    B.remove_node(node_to_remove)

# Check and ensure the graph is connected
if not nx.is_connected(B):
    components = list(nx.connected_components(B))
    if len(components) > 1:
        for i in range(len(components) - 1):
            node1 = list(components[i])[0]
            node2 = list(components[i + 1])[0]
            B.add_edge(node1, node2)

# Calculate centrality metrics
degree_centrality = nx.degree_centrality(B)
betweenness_centrality = nx.betweenness_centrality(B)

# Define node colors based on bipartite set
color_map = {
    0: 'lightblue',  # Light blue for constructors
    1: 'lightgreen'  # Light green for drivers
}

# Create separate lists for node colors and sizes
node_colors = [color_map[B.nodes[node]['bipartite']] for node in B.nodes()]
node_sizes = [degree_centrality[node] * 1000 for node in B.nodes()]  # Scale size for visibility

# Draw the graph
plt.figure(figsize=(16, 12))
pos = nx.bipartite_layout(B, nodes=constructors_set)

# Draw nodes
nx.draw_networkx_nodes(B, pos, nodelist=constructors_set, node_color='lightblue', node_size=node_sizes, label='Constructors')
nx.draw_networkx_nodes(B, pos, nodelist=drivers_set, node_color='lightgreen', node_size=node_sizes, label='Drivers')

# Draw edges
nx.draw_networkx_edges(B, pos)

# Draw labels
nx.draw_networkx_labels(B, pos, font_size=10, font_weight='bold')

# Add a legend
import matplotlib.lines as mlines
constructors_legend = mlines.Line2D([], [], color='lightblue', marker='o', linestyle='None', markersize=10, label='Constructors')
drivers_legend = mlines.Line2D([], [], color='lightgreen', marker='o', linestyle='None', markersize=10, label='Drivers')
plt.legend(handles=[constructors_legend, drivers_legend], loc='upper left', fontsize=12)

# Add a multiline title
plt.title('Bipartite Graph of Constructors and Drivers\nwith Centrality Metrics', fontsize=16, fontweight='bold')

# Adjust layout to fit everything nicely
plt.subplots_adjust(left=0.05, right=0.95, top=0.9, bottom=0.05)

# Display the plot
plt.show()

```
![download](https://github.com/user-attachments/assets/4764471f-b57a-40c5-8794-9e18a837b5c7)


## **Conclusion**

```python
-import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Load the CSV files
constructors = pd.read_csv('data/constructors.csv')
drivers = pd.read_csv('data/drivers.csv')

# Sample data preparation
top_constructors = constructors.head(10)  # Select top 10 constructors
top_drivers = drivers.head(20)            # Select top 20 drivers

# Create a Bipartite Graph
B = nx.Graph()

# Define the sets of nodes
constructors_set = top_constructors['name'].tolist()
drivers_set = top_drivers['driverRef'].tolist()

# Add nodes with bipartite set attributes
B.add_nodes_from(constructors_set, bipartite=0)  # Set 0 for constructors
B.add_nodes_from(drivers_set, bipartite=1)       # Set 1 for drivers

# Add edges between the two sets
for constructor in constructors_set:
    for driver in drivers_set:
        B.add_edge(constructor, driver)

# Check and ensure the graph is connected
if not nx.is_connected(B):
    components = list(nx.connected_components(B))
    if len(components) > 1:
        for i in range(len(components) - 1):
            node1 = list(components[i])[0]
            node2 = list(components[i + 1])[0]
            B.add_edge(node1, node2)

# Calculate centrality metrics
degree_centrality = nx.degree_centrality(B)
betweenness_centrality = nx.betweenness_centrality(B)

# Define node colors based on bipartite set
color_map = {
    0: 'lightblue',  # Light blue for constructors
    1: 'lightgreen'  # Light green for drivers
}

# Create separate lists for node colors and sizes
node_colors = [color_map[B.nodes[node]['bipartite']] for node in B.nodes()]
node_sizes = [degree_centrality[node] * 1000 for node in B.nodes()]  # Scale size for visibility

# Draw the graph
plt.figure(figsize=(16, 12))
pos = nx.bipartite_layout(B, nodes=constructors_set)

# Draw nodes
nx.draw_networkx_nodes(B, pos, nodelist=constructors_set, node_color='lightblue', node_size=[degree_centrality[node] * 1000 for node in constructors_set], label='Constructors')
nx.draw_networkx_nodes(B, pos, nodelist=drivers_set, node_color='lightgreen', node_size=[degree_centrality[node] * 1000 for node in drivers_set], label='Drivers')

# Draw edges
nx.draw_networkx_edges(B, pos, alpha=0.5)

# Draw labels
nx.draw_networkx_labels(B, pos, font_size=8, font_weight='normal')

# Add metrics annotations
for node in B.nodes():
    plt.text(pos[node][0], pos[node][1], 
             f'{node}\nDeg: {degree_centrality[node]:.2f}\nBetw: {betweenness_centrality[node]:.2f}', 
             fontsize=8, ha='center', bbox=dict(facecolor='white', alpha=0.6, edgecolor='black'))

# Title and legend
plt.title('Bipartite Graph of Constructors and Drivers with Metrics\n'
          'Average Clustering Coefficient: 0.0000\n'
          , 
          fontsize=14, fontweight='normal')
plt.legend(loc='upper left', fontsize=12)

# Display the plot
plt.show()

```
![download](https://github.com/user-attachments/assets/8652918d-bf6d-4219-a90a-4d68c288e0c4)

