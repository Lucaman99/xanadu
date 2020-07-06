## General Outline for QAOA ADR

- Introduction to QAOA/Why it is important
- Introduce different parts of QAOA
- Start by discussing how the main component of QAOA is the ability to create cost and mixer layers
- One should be able to specify a cost Hamiltonian and get a cost layer
  - Passed into the circuit in terms of a Pennylane Hamiltonian object
  - Each layer is decomposed and exponentiated into a MultiRZ gate
- One should be able to specify a mixer Hamiltonian and get a mixer layer
  - Also passed into the circuit as a Pennylane Hamiltonian object
  - Also decomposed and exponentiated
- One should then be able to take these cost and mixer layers, and pass them into a method that creates a QAOA circuit
- This circuit can then be passed into a QNode, which would be executed and optimized in the usual way

- It is oftentimes hard to come up with a cost Hamiltonian corresponding to a optimization problem
- It is also hard to choose the correct mixer Hamiltonian
- Solution --> The module will have a collection of "Optimization Problems" to choose from, which are 
  composed of a cost Hamiltonian and a reccomended mixer Hamiltonian (and possibly a reccomended initialization circuit)
  - These cost and mixer Hamiltonians can then be used to create cost and mixer layers as was done before
- In addition to this, the user would have access to a collection of commonly-used mixer Hamiltonians, which they can 
  easily call and turn into mixer layers
