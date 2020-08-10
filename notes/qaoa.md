# QAOA Tutorial Notes
---------------------

Notes/outline for a new PennyLane QAOA tutorial, incorporating the new PennyLane functionality.

### Part 1: What is QAOA?

- We should have a brief exposition on QAOA, talking more about why it is important rather than 
  spending too much time talking about all of the technical details.

### Part 2: Creating a QAOA Workflow in PennyLane

- Stress how easy it is to define cost and mixer Hamiltonians in PennyLane (we can do this with 
  only **one line of code** for any of the built-in problem instances). It might be nice to show 
  that we are making this process much more covenient for users by showing what the cost/mixer Hamiltonians 
  look like for large systems. This would be effective with MaxCut, but 10x more effective for TSP.

- Another reason for showing-off TSP in this tutorial rather than MaxCut: The QML gallery already has 
  an entry on QAOA MaxCut, so having some variety might be nice, even though the implementations will be different.

- All together, a QAOA workflow for TSP will look something like:

  ``` python

  import pennylane as qml
  from pennylane import qaoa
  import networkx as nx
  import numpy as np

  # Defines the graph and the grid of wires

  graph = nx.Graph()
  graph.add_edges_from([(0, 1, weight=0.4), (1, 2, weight=1.7)])

  wire_grid = np.array([[0, 1, 2], [3, 4, 5], [6, 7, 8]])

  # Defines the cost and mixer Hamiltonians

  cost_h, mixer_h = qaoa.travelling_salesman(graph, wire_grid)

  # Defines the QAOA layer

  def qaoa_layer(gamma, alpha):

      qaoa.cost_layer(gamma, cost_h)
      qaoa.mixer_layer(alpha, mixer_h)

  # Defines the device and the full QAOA ansatz

  dev = qml.device('default.qubit', wires=9)

  def qaoa_ansatz(params, **kwargs):

      gamma = params[0]
      alpha = params[1]
 
      qml.repeat(qaoa_layer, 3, gamma, alpha)

  # Defines the cost function

  cost_function = qml.VQECost(qaoa_ansatz, cost_h, dev)

  # Optimizes the cost function and prints the optimal parameters

  optimizer = qml.GradientDescentOptimizer()
  steps = 100
  params = [[1.0, 1.0, 1.0], [1.0, 1.0, 1.0]]

  for i in range(steps):
      optimizer.step(cost_function, params)

  print(params)

```
