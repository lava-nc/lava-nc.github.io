Neuromorphic Constraint Optimization Library
============================================

**A library of solvers that leverage neuromorphic hardware for constrained optimization.**

Constrained optimization searches for the values of input variables that minimize or maximize a given objective function, while the variables are subject to constraints. This kind of problem is ubiquitous throughout scientific domains and industries.
Constrained optimization is a promising application for neuromorphic computing as it `naturally aligns with the dynamics of spiking neural networks <https://doi.org/10.1109/JPROC.2021.3067593>`_. When individual neurons represent states of variables, the neuronal connections can directly encode constraints between the variables: In its simplest form, recurrent inhibitory synapses connect neurons which represent mutually exclusive variable states, while recurrent excitatory synapses link neurons representing reinforcing states. Implemented on massively parallel neuromorphic hardware, such a spiking neural network can simultaneously evaluate conflicts and cost functions involving many variables, and update all variables accordingly. This allows a quick convergence towards an optimal state. In addition, the fine-scale timing dynamics of SNNs allow them to readily escape from local minima.

This Lava repository provides constraint optimization solvers that leverage the benefits of neuromorphic computing for the following problems: 


#. Constraint Satisfaction Problems (CSP).
#. Quadratic Unconstrained Binary Optimization (QUBO).

In the future, the library will be extended by solvers targeting further constraint optimization problems that are relevant for robotics and operations research.
The current focus lies on solvers for the following problems:


#. Linear Programming (LP).
#. Integer Programming (IP).
#. Quadratic Programming (QP).
#. Mixed-Integer Linear Programming (MILP).
#. Mixed-Integer Quadratic Programming (MIQP).

.. image:: https://user-images.githubusercontent.com/86950058/135390275-f0c75a18-4b2f-4340-b1af-e0530259eabf.png
   :target: https://user-images.githubusercontent.com/86950058/135390275-f0c75a18-4b2f-4340-b1af-e0530259eabf.png
   :alt: Overview_Solvers3

Example
-------

.. code-block:: python

   from lava.lib.optimization import CspSolver

   variables = ['var1', 'var2', 'var3']
   domains = dict(var1 = {0, 1, 2}, var2 = {'a', 'b', 'c'}, var3 ={'red', 'blue', 'green'})
   solver = CspSolver(problem=(variables, domains, constraints))
   solution, t_sol = solver.solve(timeout=5000, backend='Loihi1')

.. code-block:: python

   solution, t_sol = solver.solve(timeout=5000, backend='Loihi2', profile=True)
   print(solver.time_to_solution)
   print(solver.energy_to_solution)
