.. Lava documentation master file, created by
   sphinx-quickstart on Tue Sep 28 14:54:33 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Lava Documentation
==================

.. role:: raw-html-m2r(raw)
   :format: html



.. image:: https://user-images.githubusercontent.com/68661711/135301797-400e163d-71a3-45f8-b35f-e849e8c74f0c.png
   :target: https://user-images.githubusercontent.com/68661711/135301797-400e163d-71a3-45f8-b35f-e849e8c74f0c.png
   :alt: image



.. raw:: html

   <p align="center"><b>
     A software framework for neuromorphic computing
   </b></p>


Introduction
============

Lava is an open-source software framework for developing neuro-inspired applications and mapping them to neuromorphic hardware. It intrinsically leverages the advantages of neuromorphic hardware to support its goal of matching the ability of the human brain  human brain's ability to intelligently process, learn from, and respond to real-world data within milliseconds at microwatt power levels.

The vision behind Lava is a common, open, professionally developed code base that unites the full range of approaches pursued by the neuromorphic computing community. It provides a modular and composable structure for researchers to integrate their best ideas into a growing algorithms library, and it presents compelling and productive abstractions to application developers. For this purpose, Lava allows researchers to define versatile processes like individual neurons, neural networks, or interfaces to peripheral devices, and to connect it into complex neuromorphic applications. 

Lava is platform-agnostic so that applications can be tested on conventional CPUs/GPUs and deployed to a wide range of neuromorphic chips such as Intel's Loihi. To compile and execute processes for different backends, Lava builds on a low-level interface called Magma with a powerful compiler and runtime library. This framework natively supports:

#. Massive parallelism.
#. Channel-based message passing between asynchronous processes using Communicating Sequential Processes.
#. Heterogeneous execution platforms with both conventional and neuromorphic components.
#. Real-time and Offline training.
#. Measurement of performance and energy consumption.
#. Integration with third-party frameworks.

        
For users, Lava blends a simple Python Interface with an excellent performance due to underlying C/C++/CUDA/OpenCL code.

For more information, visit the Lava Documentation: https://lava-nc.github.io/

Release plan
============

Lava has originally been developed by Intel's Neuromorphic Computing Lab but will be completely open-sourced in stages beginning October 2021 together with the announcement of Intel's new Loihi 2 neuromorphic processor architecture.
During the first two months after the initial release, there will be rapid bi-weekly releases of the core Lava components and first algorithm libraries by Intel. 

After this first wave of releases, Intel will slow down to a quarterly release schedule of new features and enhancements. At the same time, increasing engagement with open-source contributors is expected.

**Initial release schedule:**


+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Component                      | HW Support  | Features                                                                                                                                                                                                                                                         |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Magma                          | CPU, GPU    | - The generic high-level and HW-agnostic API supports creation of processes that execute asynchronously, in parallel and communicate via messages over channels to enable algorithm and application development.                                                 |
|                                |             | - Compiler and Run time initially only support execution or simulation on CPU and GPU platform.                                                                                                                                                                  |
|                                |             | - A series of basic examples and tutorials explain Lava's key architectural and usage concepts                                                                                                                                                                   |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Process library                | CPU, GPU    | Process library initially supports basic processes to create spiking neural networks with different neuron models, connection topologies and input/output processes.                                                                                             |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deep Learning library          | CPU, GPU    | The Lava Deep Learning (DL) library allows for direct training of stateful and event-based spiking neural networks with backpropagation via SLAYER 2.0 as well as inference through Lava. Training and inference will initially only be supported on CPU/GPU HW. |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Optimization library           | CPU, GPU    | The Lava optimization library offers a variety of constraint optimization solvers such as constraint satisfaction (CSP) or quadratic unconstraint binary optimization (QUBO) and more.                                                                           |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dynamic Neural Field library   | CPU, GPU    | The Dynamic Neural Field (DNF) library allows to build neural attractor networks for working memory, decision making, basic neuronal representations, and learning.                                                                                              |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Magma and Process library      | Loihi 1, 2  | Compiler, Runtime and the process library will be upgraded to support Loihi 1 and 2 architectures.                                                                                                                                                               |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Profiler                       | CPU, GPU    | The Lava Profiler enable power and performance measurements on neuromorphic HW as well as the ability to simulate power and performance of neuromorphic HW on CPU/GPU platforms. Initially only CPU/GPU support will be available.                               |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| DL, DNF & Optimization library | Loihi 1, 2  | All algorithm libraries will be upgraded to support and be properly tested on neuromorphic HW.                                                                                                                                                                   |
+--------------------------------+-------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+



Lava organization
=================

... Show figure of SW stack

... Explain the different repositories and components in tabular form and related to SW tack



Getting started
===============

Install instructions
--------------------

Installing or cloning Lava
^^^^^^^^^^^^^^^^^^^^^^^^^^

New Lava releases will be published via GitHub releases and can be installed after downloading.

.. code-block:: console


      pip install lava-0.0.1.tar.gz
      pip install lava-lib-0.0.1.tar.gz

If you would like to contribute to the source code or work with the source directly, you can also clone the repository.

.. code-block:: console


      git clone git@github.com:lava-nc/lava.git
      pip install -e lava/lava

      git clone git@github.com:lava-nc/lava-lib.git
      # [Optional]
      pip install -e lava-lib/dnf
      pip install -e lava-lib/dl
      pip install -e lava-lib/optimization

This will allow you to run Lava on your own local CPU or GPU.

Running Lava on Intel Loihi
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Intel's neuromorphic Loihi 1 or 2 research systems are currently not available commercially. Developers interested in using Lava with Loihi systems, need to join the Intel Neuromorphic Research Community (INRC). Once a member of the INRC, developers will gain access to cloud-hosted Loihi systems or are able obtain physical Loihi systems on a loan basis as well as additional proprietary components of the magma library which enables to compile processes for Loihi systems.

Please email inrc_interest@intel.com to request a template to apply for INRC membership.

.. code-block:: console


      pip install /nfs/ncl/releases/lava/0.0.1/lava-nc-0.0.1.tar.gz
      pip install /nfs/ncl/releases/lava/0.0.1/lava-nc-lib-0.0.1.tar.gz

Coding example
--------------

Building a simple feed-forward network
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

   # Instantiate Lava processes to build network
   import numpy as np
   from lava.proc.io import SpikeInput, SpikeOutput
   from lava.proc import Dense, LIF

   si = SpikeInput(path='source_data_path', shape=(28, 28))
   dense = Dense(in_shape=(28, 28),
                 out_shape=(10, 1),
                 weights=np.random.random((28, 28)))
   lif = LIF(shape=(10,), vth=10)
   so = SpikeOutput(path='result_data_path', shape=(10,))

   # Connect processes via their directional input and output ports
   si.out_port.s_out.connect(dense.in_ports.s_in)
   dense.out_port.a_out.connect(lif.in_ports.a_in)
   lif.out_port.s_out.connect(so.in_ports.s_in)

   # Execute processes for fixed number of steps on Loihi 2 (by running any of them)
   from lava.magma import run_configs as rcfg
   from lava.magma import run_conditions as rcnd
   lif.run(run_cfg=rcfg.Loihi2HwCfg(),
           condition=rcnd.RunSteps(1000, blocking=True))

Creating a custom Lava process
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A process has input and output ports to interact with other processes, internal variables may have different behavioral implementations in different programming languages or for different HW platforms.

.. code-block:: python

   from lava.magma import AbstractProcess, InPort, Var, OutPort

   class LIF(AbstractProcess):
       """Leaky-Integrate-and-Fire neural process with activation input and spike
       output ports a_in and s_out.

       Realizes the following abstract behavior:
       u[t] = u[t-1] * (1-du) + a_in
       v[t] = v[t-1] * (1-dv) + u[t] + b
       s_out = v[t] > vth
       v[t] = v[t] - s_out*vth
       """
       def __init__(self, **kwargs):
           super(AbstractProcess, self).__init__(kwargs)
           shape = kwargs.pop("shape", (1,))
           # Declare input and output ports
           self.a_in = InPort(shape=shape)
           self.s_out = OutPort(shape=shape)
           # Declare internal variables
           self.u = Var(shape=shape, init=0)
           self.v = Var(shape=shape, init=0)
           self.decay_u = Var(shape=(1,), init=kwargs.pop('du', 1))
           self.decay_v = Var(shape=(1,), init=kwargs.pop('dv', 0))
           self.b = Var(shape=shape, init=kwargs.pop('b', 0))
           self.vth = Var(shape=(1,), init=kwargs.pop('vth', 1))

Creating process models
^^^^^^^^^^^^^^^^^^^^^^^

Process models are used to provide different behavioral models of a process. This Python model implements the LIF process, the Loihi synchronization protocol and requires a CPU compute resource to run.

.. code-block:: python

   import numpy as np
   from lava import magma as mg
   from lava.magma.resources import CPU
   from lava.magma.sync_protocol import LoihiProtocol, DONE
   from lava.proc import LIF
   from lava.magma.pymodel import AbstractPyProcessModel, LavaType
   from lava.magma.pymodel import InPortVecDense as InPort
   from lava.magma.pymodel import OutPortVecDense as OutPort

   @mg.implements(proc=LIF, protocol=LoihiProtocol)
   @mg.requires(CPU)
   class PyLifModel(AbstractPyProcessModel):
       # Declare port implementation
       a_in: InPort =     LavaType(InPort, np.int16, precision=16)
       s_out: OutPort =   LavaType(OutPort, bool, precision=1)
       # Declare variable implementation
       u: np.ndarray =    LavaType(np.ndarray, np.int32, precision=24)
       v: np.ndarray =    LavaType(np.ndarray, np.int32, precision=24)
       b: np.ndarray =    LavaType(np.ndarray, np.int16, precision=12)
       du: int =          LavaType(int, np.uint16, precision=12)
       dv: int =          LavaType(int, np.uint16, precision=12)
       vth: int =         LavaType(int, int, precision=8)

       def run_spk(self):
           """Executed during spiking phase of synchronization protocol."""
           # Decay current
           self.u[:] = self.u * (1 - self.du)
           # Receive input activation via channel and accumulate
           activation = self.a_in.recv()
           self.u[:] += activation
           self.v[:] = self.v * (1 - self.dv) + self.u + self.b
           # Generate output spikes and send to receiver
           spikes = self.v > self.vth
           self.v[spikes] -= self.vth
           if np.any(spikes):
               self.s_out.send(spikes)

In contrast this process model also implements the LIF process but by structurally allocating neural network resources on a virtual Loihi 1 neuro core.

.. code-block:: python

   from lava import magma as mg
   from lava.magma.resources import Loihi1NeuroCore
   from lava.proc import LIF
   from lava.magma.ncmodel import AbstractNcProcessModel, LavaType, InPort, OutPort, Var

   @mg.implements(proc=LIF)
   @mg.requires(Loihi1NeuroCore)
   class NcProcessModel(AbstractNcProcessModel):
       # Declare port implementation
       a_in: InPort =   LavaType(InPort, precision=16)
       s_out: OutPort = LavaType(OutPort, precision=1)
       # Declare variable implementation
       u: Var =         LavaType(Var, precision=24)
       v: Var =         LavaType(Var, precision=24)
       b: Var =         LavaType(Var, precision=12)
       du: Var =        LavaType(Var, precision=12)
       dv: Var =        LavaType(Var, precision=12)
       vth: Var =       LavaType(Var, precision=8)

       def allocate(self, net: mg.Net):
           """Allocates neural resources in 'virtual' neuro core."""
           num_neurons = self.in_args['shape'][0]
           # Allocate output axons
           out_ax = net.out_ax.alloc(size=num_neurons)
           net.connect(self.s_out, out_ax)
           # Allocate compartments
           cx_cfg = net.cx_cfg.alloc(size=1,
                                     du=self.du,
                                     dv=self.dv,
                                     vth=self.vth)
           cx = net.cx.alloc(size=num_neurons,
                             u=self.u,
                             v=self.v,
                             b_mant=self.b,
                             cfg=cx_cfg)
           cx.connect(out_ax)
           # Allocate dendritic accumulators
           da = net.da.alloc(size=num_neurons)
           da.connect(cx)
           net.connect(self.a_in, da)

Communication
=============


* Why to reach out?
* Link to newsletter subscription goes here to learn more about latest lava developments and releases

Documentation Overview
======================

.. toctree::
   :maxdepth: 2

   lava_architecture_overview
   lava_api_documentation
   tutorials
   algorithms
   developer_guide
   loihi_overview



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
