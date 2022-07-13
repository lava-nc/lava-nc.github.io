.. Lava documentation master file, created by
   sphinx-quickstart on Tue Sep 28 14:54:33 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Lava Software Framework
=======================

.. role:: raw-html-m2r(raw)
   :format: html

|

.. image:: https://user-images.githubusercontent.com/68661711/135301797-400e163d-71a3-45f8-b35f-e849e8c74f0c.png
   :target: https://user-images.githubusercontent.com/68661711/135301797-400e163d-71a3-45f8-b35f-e849e8c74f0c.png
   :alt: image
   :align: center

|

.. raw:: html

   <p align="center"><b>
     A software framework for neuromorphic computing
   </b></p>


Introduction
============

Lava is an open-source software framework for developing neuro-inspired applications and mapping them to neuromorphic hardware. Lava provides developers with the tools and abstractions to develop applications that fully exploit the principles of neural computation.  Constrained in this way, like the brain, Lava applications allow neuromorphic platforms to intelligently process, learn from, and respond to real-world data with great gains in energy efficiency and speed compared to conventional computer architectures.

The vision behind Lava is an open, community-developed code base that unites the full range of approaches pursued by the neuromorphic computing community. It provides a modular, composable, and extensible structure for researchers to integrate their best ideas into a growing algorithms library, while introducing new abstractions that allow others to build on those ideas without having to reinvent them.

For this purpose, Lava allows developers to define versatile *processes* such as individual neurons, neural networks, conventionally coded programs, interfaces to peripheral devices, and bridges to other software frameworks. Lava allows collections of these processes to be encapsulated into modules and aggregated to form complex neuromorphic applications.  Communication between Lava processes uses event-based message passing, where messages can range from binary spikes to kilobyte-sized packets.

The behavior of Lava processes is defined by one or more *implementation models*\ , where different models may be specified for different execution platforms ("backends"), different degrees of precision, and for high-level algorithmic modeling purposes.  For example, an excitatory/inhibitory neural network process may have different implementation models for an analog neuromorphic chip compared to a digital neuromorphic chip, but the two models could share a common "E/I" process definition with each model's implementations determined by common input parameters.

Lava is platform-agnostic so that applications can be prototyped on conventional CPUs/GPUs and deployed to heterogeneous system architectures spanning both conventional processors as well as a range of neuromorphic chips such as Intel's Loihi. To compile and execute processes for different backends, Lava builds on a low-level interface called *Magma* with a powerful compiler and runtime library. Over time, the Lava developer community may enhance Magma to target additional neuromorphic platforms beyond its initial support for Intel's Loihi chips.

The Lava framework currently supports (to be released soon):

#. Channel-based message passing between asynchronous processes (the Communicating Sequential Processes paradigm)
#. Hyper-granular parallelism where computation emerges as the collective result of inter-process interactions
#. Heterogeneous execution platforms with both conventional and neuromorphic components
#. Offline backprop-based training of a wide range of neuron models and network topologies
#. Tools for generating complex spiking neural networks such as *dynamic neural fields* and networks that solve well-defined optimization problems
#. Integration with third-party frameworks

For maximum developer productivity, Lava blends a simple Python Interface with accelerated performance using underlying C/C++/CUDA code.

For more information, visit Lava on Github: `https://github.com/lava-nc <https://github.com/lava-nc>`__


Lava organization
=================

Processes are the fundamental building block in the Lava architecture from which all algorithms and applications are built. Processes are stateful objects with internal variables, input and output ports for message-based communication via channels and multiple behavioral models. This architecture is inspired from the Communicating Sequential Process (CSP) paradigm for asynchronous, parallel systems that interact via message passing. Lava processes implementing the CSP API can be compiled and executed via a cross-platform compiler and runtime that support execution on neuromorphic and conventional von-Neumann HW. Together, these components form the low-level Magma layer of Lava.

At a higher level, the process library contains a growing set of generic processes that implement various kinds of neuron models, neural network connection topologies, IO processes, etc. These execute on either CPU, GPU or neuromorphic HW such as Intel's Loihi architecture. 

Various algorithm and application libraries build on these these generic processes to create specialized processes and provide tools to train or configure processes for more advanced applications. A deep learning library, constrained optimization library, and dynamic neural field library are among the first to be released in Lava, with more libraries to come in future releases.

Lava is open to modification and extension to third-party libraries like Nengo, ROS, YARP and others. Additional utilities also allow users to profile power and performance of workloads, visualize complex networks, or help with the float to fixed point conversions required for many low-precision devices such as neuromorphic HW.


.. image:: https://user-images.githubusercontent.com/68661711/135412508-4a93e20a-8b64-4723-a69b-de8f4b5902f7.png
   :target: https://user-images.githubusercontent.com/68661711/135412508-4a93e20a-8b64-4723-a69b-de8f4b5902f7.png
   :alt: image


All of Lava's core APIs and higher-level components are released, by default, with permissive BSD 3 licenses in order to encourage the broadest possible community contribution.  Lower-level Magma components needed for mapping processes to neuromorphic backends are generally released with more restrictive LGPL-2.1 licensing to discourage commercial proprietary forks of these technologies.  The specific components of Magma needed to compile processes specifically to Intel Loihi chips remains proprietary to Intel and is not provided through this GitHub site (see below).  Similar Magma-layer code for other future commercial neuromorphic platforms likely will also remain proprietary.

Coding example
--------------

Building a simple feed-forward network
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

   # Instantiate Lava processes to build network
   from lava.proc.dense.process import Dense
   from lava.proc.lif.process import LIF

   lif1 = LIF()
   dense = Dense()
   lif2 = LIF()

   # Connect processes via their directional input and output ports
   lif1.out_ports.s_out.connect(self.dense.in_ports.s_in)
   dense.out_ports.a_out.connect(self.lif2.in_ports.a_in)

   # Execute process lif1 and all processes connected to it for fixed number of steps
   from lava.magma.core.run_conditions import RunSteps
   from lava.magma.core.run_configs import RunConfig
   lif1.run(condition=RunSteps(num_steps=10), run_cfg=SimpleRunConfig(
            sync_domains=[]))
   lif1.stop()

Creating a custom Lava process
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A process has input and output ports to interact with other processes, internal variables may have different behavioral implementations in different programming languages or for different HW platforms.

.. code-block:: python

   from lava.magma.core.process.process import AbstractProcess
   from lava.magma.core.process.variable import Var
   from lava.magma.core.process.ports.ports import InPort, OutPort


   class LIF(AbstractProcess):
       """Leaky-Integrate-and-Fire neural process with activation input and spike
       output ports a_in and s_out.

       Realizes the following abstract behavior:
       u[t] = u[t-1] * (1-du) + a_in
       v[t] = v[t-1] * (1-dv) + u[t] + bias
       s_out = v[t] > vth
       v[t] = v[t] - s_out*vth
       """
       def __init__(self, **kwargs):
          super().__init__(**kwargs)
          shape = kwargs.get("shape", (1,))
          self.a_in = InPort(shape=shape)
          self.s_out = OutPort(shape=shape)
          self.u = Var(shape=shape, init=0)
          self.v = Var(shape=shape, init=0)
          self.du = Var(shape=(1,), init=kwargs.pop("du", 0))
          self.dv = Var(shape=(1,), init=kwargs.pop("dv", 0))
          self.bias = Var(shape=shape, init=kwargs.pop("b", 0))
          self.vth = Var(shape=(1,), init=kwargs.pop("vth", 10))

Creating process models
^^^^^^^^^^^^^^^^^^^^^^^

Process models are used to provide different behavioral models of a process. This Python model implements the LIF process, the Loihi synchronization protocol and requires a CPU compute resource to run.

.. code-block:: python

   import numpy as np
   from lava.magma.core.sync.protocols.loihi_protocol import LoihiProtocol
   from lava.magma.core.model.py.ports import PyInPort, PyOutPort
   from lava.magma.core.model.py.type import LavaPyType
   from lava.magma.core.resources import CPU
   from lava.magma.core.decorator import implements, requires
   from lava.magma.core.model.py.model import PyLoihiProcessModel
   from lava.proc.lif.process import LIF


   @implements(proc=LIF, protocol=LoihiProtocol)
   @requires(CPU)
   class PyLifModel(PyLoihiProcessModel):
       a_in: PyInPort = LavaPyType(PyInPort.VEC_DENSE, np.int16, precision=16)
       s_out: PyOutPort = LavaPyType(PyOutPort.VEC_DENSE, bool, precision=1)
       u: np.ndarray = LavaPyType(np.ndarray, np.int32, precision=24)
       v: np.ndarray = LavaPyType(np.ndarray, np.int32, precision=24)
       bias: np.ndarray = LavaPyType(np.ndarray, np.int16, precision=12)
       du: int = LavaPyType(int, np.uint16, precision=12)
       dv: int = LavaPyType(int, np.uint16, precision=12)
       vth: int = LavaPyType(int, int, precision=8)

       def run_spk(self):
           self.u[:] = self.u * ((2 ** 12 - self.du) // 2 ** 12)
           a_in_data = self.a_in.recv()
           self.u[:] += a_in_data
           self.v[:] = self.v * \
               ((2 ** 12 - self.dv) // 2 ** 12) + self.u + self.bias
           s_out = self.v > self.vth
           self.v[s_out] = 0  # Reset voltage to 0. This is Loihi-1 compatible.
           self.s_out.send(s_out)

Stay in touch
=============

To receive regular updates on the latest developments and releases of the Lava Software Framework please `subscribe to our newsletter <http://eepurl.com/hJCyhb>`_.



Documentation Overview
======================

.. toctree::
   :maxdepth: 2

   lava_architecture_overview
   getting_started_with_lava
   algorithms
   developer_guide
   lava_api_documentation





Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
