Dynamic Neural Fields
=====================


.. image:: https://user-images.githubusercontent.com/5708333/135443996-7492b968-277a-4397-9b1c-597b7af4a699.gif
   :target: https://user-images.githubusercontent.com/5708333/135443996-7492b968-277a-4397-9b1c-597b7af4a699.gif
   :alt: 2D DNF tracking bias input
:raw-html-m2r:`<br>`
*Voltage of a selective dynamic neural field tracking input*


Introduction
------------

Dynamic Neural Fields (DNF) are neural attractor networks that generate stabilized activity patterns in recurrently connected populations of neurons. These activity patterns form the basis for working memory, decision making, basic neuronal representations, and learning.  DNFs are the fundamental building block of Dynamic Field Theory (DFT, Home | Dynamic field theory), a mathematical and conceptual framework for modeling cognitive processes in the embodied cognition approach, i.e. cognition (memory, representation, decision making, and learning) in a closed behavioral loop. 

What is lava-dnf?
-----------------

lava-dnf is a Lava library, an open-source software framework for neuromorphic computing. The main building block in Lava are processes. lava-dnf provides processes and other software infrastructure to build architectures composed of DNFs (attractor networks) as the main building blocks. 
This library also provides tools to direct sensory input to the neuronal architecture and to read out the, e.g. motor control, output.  

The primary focus of lava-dnf today are robotic applications - sensing and perception, motion control, behavioral organization, map formation, and autonomous (continual) learning. Neuromorphic hardware provides significant gains in both processing speed and energy efficiency compared to conventional implementations of DNFs on a CPU or GPU (e.g. using cedar or cosivina).

Key features
------------

Building DNF architectures
    • Based on spiking neurons
    • DNF dimensionality support for 0D, 1D, 2D, and 3D
    • recurrent connectivity based on kernel functions
    • forward connectivity to connect multiple DNFs
    • structured input from spike generators

Examples demonstrating basic DNF regimes and instabilities
    • detection of input
    • selection of input
    • working memory of input
    • neural oscillator

Infrastructure
    • Sensor and data input/output
    • Plotting


Example
-------

.. code-block:: python

   from lava.lib.dnf.populations import Population
   from lava.lib.dnf.kernels import SelectiveKernel
   from lava.lib.dnf.connect import connect
   from lava.lib.dnf.operations import Instar, OneToOne

   # create population of 20x20 spiking neurons
   dnf = Population(shape=(20, 20))

   # create a selective kernel
   kernel = SelectiveKernel(amp_exc=18, width_exc=[4, 4], global_inh=-15)

   # apply the kernel to the population to create a DNF with a selective regime
   connect(dnf, dnf, [Instar(kernel)])
