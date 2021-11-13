Getting Started With Lava
=========================

This guide to programming Lava will provide a growing collection of learning resources to help you become a Lava developer! It will cover all aspects of Lava enabling you to create, compile, execute and understand Lava *Processes*. Review the `Lava Architecture <https://lava-nc.org/lava_architecture_overview.html>`_ section for an introduction to the fundamental architectural concepts of Lava.

The following two sections currently include **application examples** that give a broader overview how to use Lava features in practice while the tutorials on the **fundamental concepts** provide a deeper dive into specific features of Lava and how to use them.


Application examples:
---------------------

These are standalone tutorials that can be followed in any order.

* `MNIST digit classification: <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/end_to_end/tutorial01_mnist_digit_classification.ipynb>`_ The classical '*Hello World!*' example illustrating how to build a simple feed-forward image classifier using leaky-integrate-and-fire neurons.

**Coming soon:**

* Liquid state machine: Another classical canonical example in neuromorphic computing, illustrating a complex, spiking and recurrent neural network for time series classification.

* Learning: Many neuromorphic architectures offer local on-chip learning via bio-inspired STDP learning. This tutorial provides a simple example how such learning rules can be used.


Fundamental concepts:
---------------------

These tutorials walk step-by-step through the fundamental concepts of Lava introduced in `Lava Architecture <https://lava-nc.org/lava_architecture_overview.html>`_. These tutorials tend to build on each other, therefore it's best to consume them in order.

1. `Installing Lava <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/in_depth/tutorial01_installing_lava.ipynb>`_:
Quickly get Lava installed, tested and ready to develop on your system.
  
2. `Processes <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/in_depth/tutorial02_processes.ipynb>`_:
Learn how to create *Processes* which are Lava's fundamental computational building blocks.
  
3. `Process models <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/in_depth/tutorial03_process_models.ipynb>`_:
Learn to implement *Process* behavior with the ability to run on diverse backends via Lava *ProcessModels*.

4. `Execution <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/in_depth/tutorial04_execution.ipynb>`_:
This notebook demonstrates how to configure, start, and stop the execution of a network of *Processes*.

.. 5. `Connecting processes <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/in_depth/tutorial05_connect_processes.ipynb>`_:
.. How to connect *Processes* for message-based communication via channels and to build a network of asynchronously operating and interacting *Processes*.

.. 6. `Hierarchical processes <https://github.com/lava-nc/lava/blob/main/src/lava/tutorials/in_depth/tutorial06_hierarchical_processes.ipynb>`_:
.. Processes can be composed into hierarchical processes. Learn how to implement *SubProcessModels* to build modular *Processes* of *Processes*.

**Coming shortly:**

5. Connecting processes:
How to connect *Processes* for message-based communication via channels and to build a network of asynchronously operating and interacting *Processes*.

6. Hierarchical processes:
*Processes* can be composed into hierarchical processes. Learn how to implement *SubProcessModels* to build modular *Processes* of *Processes*.

7. Direct memory access:
Explains how to realize direct memory access between *Processes* and what to be cautious about.

8. State monitoring:
Lava offers probes to monitor the evolution of temporal state during execution. Learn how to create probes, retrieve timeseries and how to create custom visualization of network dynamics.
