Deep Learning
=============

Introduction
------------

Lava-DL (``lava-dl``) is a library of deep learning tools within Lava that 
support offline training, online training and inference methods for
various Deep Event-Based Networks.

There are two main strategies for training Deep Event-Based Networks:
*direct training* and *ANN to SNN converison*.

Directly training the network utilizes the information of precise timing of events. Direct training is very accurate and results in efficient networks. However, directly training networks take a lot of time and resources.

On the other hand, ANN to SNN conversion is especially suitable for rate
coded SNNs where we can leverage the fast training of ANN. These
converted SNNs, however, require increased latency compared to directly
trained SNNs.

Lava-DL provides an improved version of
`SLAYER <https://github.com/bamsumit/slayerPytorch>`__ for direct
training of deep event based networks and a new ANN-SNN accelerated
training approach called `Bootstrap <lava-lib-dl/bootstrap/bootstrap.html>`__ to mitigate high latency issue of conventional ANN-SNN methods for training Deep
Event-Based Networks.

The lava-dl training libraries are independent of the core lava library since Lava Processes cannot be trained directly at this point. Instead, lava-dl is first used to train the model which can then be converted to a network of Lava processes using the netx library using platform independent hdf5 network description.

The library presently consists of

1. ``lava.lib.dl.slayer`` for natively training Deep Event-Based
   Networks.
2. ``lava.lib.dl.bootstrap`` for training rate coded SNNs.

Coming soon to the library 1. ``lava.lib.dl.netx`` for training and
deployment of event-based deep neural networks on traditional as well as
neuromorphic backends.

More tools will be added in the future.

Lava-DL Workflow
----------------

.. raw:: html

   <p align="center">
   <img src="https://user-images.githubusercontent.com/29907126/140595634-a97886c6-280a-4771-830b-ae47a9324612.png" alt="Drawing" style="max-height: 400px;"/>
   </p>

Typical Lava-DL workflow consists of: 

* **Training:** using
  ``lava.lib.dl.{slayer/bootstrap}`` which results in a *hdf5 network
  description*. Training usually consists of iterative cycle of
  architecture design, hyperparameter tuning, and backpropagation
  training. 
* **Inference:** using ``lava.lib.dl.netx`` which generates
  lava proces from the hdf5 network description of the trained network and
  enables inference on different backends.

Getting Started
---------------

**End to end training tutorials**

* `Oxford spike train regression <lava-lib-dl/slayer/notebooks/oxford/train.html>`__
* `MNIST digit classification <lava-lib-dl/bootstrap/notebooks/mnist/train.html>`__ 
* `NMNIST digit classification <lava-lib-dl/slayer/notebooks/nmnist/train.html>`__ 
* `PilotNet steering angle prediction <lava-lib-dl/slayer/notebooks/pilotnet/train.html>`__

**Deep dive tutorials**

* `Dynamics and Neurons <lava-lib-dl/slayer/notebooks/neuron_dynamics/dynamics.html>`__

**Inference tutorials**

* `Oxford Inference <lava-lib-dl/netx/notebooks/oxford/run.html>`__
* `PilotNet SNN Inference <lava-lib-dl/netx/notebooks/pilotnet_snn/run.html>`__
* `PilotNet SDNN Inference <lava-lib-dl/netx/notebooks/pilotnet_sdnn/run.html>`__

SLAYER 2.0
----------

SLAYER 2.0 (`lava.lib.dl.slayer`) is an enhanced version of
`SLAYER <https://github.com/bamsumit/slayerPytorch>`__. Most noteworthy
enhancements are: support for *recurrent network structures*, a wider
variety of *neuron models* and *synaptic connections* (a complete list
of features is
`here <lava-lib-dl/slayer/slayer.html>`_).
This version of SLAYER is built on top of the
`PyTorch <https://pytorch.org/>`__ deep learning framework, similar to
its predecessor. For smooth integration with Lava,
`lava.lib.dl.slayer` supports exporting trained models using the
platform independent **hdf5 network exchange** format.

In future versions, SLAYER will get completely integrated into Lava to
train Lava Processes directly. This will eliminate the need for
explicitly exporting and importing the trained networks.

Example Code
~~~~~~~~~~~~

**Import modules**

.. code:: python

   import lava.lib.dl.slayer as slayer

**Network Description**

.. code:: python

   # like any standard pyTorch network
   class Network(torch.nn.Module):
       def __init__(self):
           ...
           self.blocks = torch.nn.ModuleList([# sequential network blocks 
                   slayer.block.sigma_delta.Input(sdnn_params), 
                   slayer.block.sigma_delta.Conv(sdnn_params,  3, 24, 3),
                   slayer.block.sigma_delta.Conv(sdnn_params, 24, 36, 3),
                   slayer.block.rf_iz.Conv(rf_params, 36, 64, 3, delay=True),
                   slayer.block.rf_iz.Conv(sdnn_cnn_params, 64, 64, 3, delay=True),
                   slayer.block.rf_iz.Flatten(),
                   slayer.block.alif.Dense(alif_params, 64*40, 100, delay=True),
                   slayer.block.cuba.Recurrent(cuba_params, 100, 50),
                   slayer.block.cuba.KWTA(cuba_params, 50, 50, num_winners=5)
               ])

       def forward(self, x):
           for block in self.blocks: 
               # forward computation is as simple as calling the blocks in a loop
               x = block(x)
           return x

       def export_hdf5(self, filename):
           # network export to hdf5 format
           h = h5py.File(filename, 'w')
           layer = h.create_group('layer')
           for i, b in enumerate(self.blocks):
               b.export_hdf5(layer.create_group(f'{i}'))

**Training**

.. code:: python

   net = Network()
   assistant = slayer.utils.Assistant(net, error, optimizer, stats)
   ...
   for epoch in range(epochs):
       for i, (input, ground_truth) in enumerate(train_loader):
           output = assistant.train(input, ground_truth)
           ...
       for i, (input, ground_truth) in enumerate(test_loader):
           output = assistant.test(input, ground_truth)
           ...

**Export the network**

.. code:: python

   net.export_hdf5('network.net')

Bootstrap
---------

In general ANN-SNN conversion methods for rate based SNN result in high latency of the network during inference. This is because the rate interpretation of a spiking neuron using ReLU acitvation unit breaks down for short inference times. As a result, the network requires many time steps per sample to achieve adequate inference results.

Bootstrap (`lava.lib.dl.bootstrap`) enables rapid training of rate based SNNs by translating them to an equivalent dynamic ANN representation which leads to SNN performance close to the equivalent ANN and low latency inference. More details `here <lava-lib-dl/bootstrap/bootstrap.html>`__. It also supports *hybrid training*
a mixed ANN-SNN network to minimize the ANN to SNN performance gap. This method is independent of the SNN model being used.

It has similar API as `lava.lib.dl.slayer` and supports exporting
trained models using the platform independent **hdf5 network exchange**
format.

.. _example-code-1:

Example Code
~~~~~~~~~~~~

**Import modules**

.. code:: python

   import lava.lib.dl.bootstrap as bootstrap

**Network Description**

.. code:: python

   # like any standard pyTorch network
   class Network(torch.nn.Module):
       def __init__(self):
           ...
           self.blocks = torch.nn.ModuleList([# sequential network blocks 
                   bootstrap.block.cuba.Input(sdnn_params), 
                   bootstrap.block.cuba.Conv(sdnn_params,  3, 24, 3),
                   bootstrap.block.cuba.Conv(sdnn_params, 24, 36, 3),
                   bootstrap.block.cuba.Conv(rf_params, 36, 64, 3),
                   bootstrap.block.cuba.Conv(sdnn_cnn_params, 64, 64, 3),
                   bootstrap.block.cuba.Flatten(),
                   bootstrap.block.cuba.Dense(alif_params, 64*40, 100),
                   bootstrap.block.cuba.Dense(cuba_params, 100, 10),
               ])

       def forward(self, x, mode):
           ...
           for block, m in zip(self.blocks, mode):
               x = block(x, mode=m)

           return x

       def export_hdf5(self, filename):
           # network export to hdf5 format
           h = h5py.File(filename, 'w')
           layer = h.create_group('layer')
           for i, b in enumerate(self.blocks):
               b.export_hdf5(layer.create_group(f'{i}'))

**Training**

.. code:: python

   net = Network()
   scheduler = bootstrap.routine.Scheduler()
   ...
   for epoch in range(epochs):
       for i, (input, ground_truth) in enumerate(train_loader):
           mode = scheduler.mode(epoch, i, net.training)
           output = net.forward(input, mode)
           ...
           loss.backward()
       for i, (input, ground_truth) in enumerate(test_loader):
           mode = scheduler.mode(epoch, i, net.training)
           output = net.forward(input, mode)
           ...

**Export the network**

.. code:: python

   net.export_hdf5('network.net')

Network Exchange (NetX) Library 
-------------------------------

For inference using Lava, Network Exchange Library (`lava.lib.dl.netx`) provides an
automated API for loading SLAYER-trained models as Lava Processes, which
can be directly run on a desired backend. ``lava.lib.dl.netx`` imports
models saved via SLAYER using the hdf5 network exchange format. The
details of hdf5 network description specification can be found
`here <lava-lib-dl/netx/netx.html>`__.

.. _example-code-2:

Example Code
~~~~~~~~~~~~

**Import modules**

.. code:: python

   from lava.lib.dl.netx import hdf5

**Load the trained network**

.. code:: python

   # Import the model as a Lava Process
   net = hdf5.Network(net_config='network.net')

**Attach Processes for Input Injection and Output Readout**

.. code:: python

   from lava.proc.io import InputLoader, BiasWriter, OutputReader

   # Instantiate the processes
   input_loader = InputLoader(dataset=testing_set)
   bias_writer = BiasWriter(shape=input_shape)
   output = OutputReader()

   # Connect the input to the network:
   input_loader.data_out.connect(bias_writer.bias_in)
   bias_writer.bias_out.connect(net.in_layer.bias)

   # Connect network-output to the output process
   net.out_layer.neuron.s_out.connect(output.net_output_in)

   from lava.proc import io   
   
   # Instantiate the processes
   dataloader = io.dataloader.SpikeDataloader(dataset=test_set)
   output_logger = io.sink.RingBuffer(shape=net.out_layer.shape, buffer=num_steps)
   gt_logger = io.sink.RingBuffer(shape=(1,), buffer=num_steps)   
   
   # Connect the input to the network:
   dataloader.ground_truth.connect(gt_logger.a_in)
   dataloader.s_out.connect(net.in_layer.neuron.a_in)   
   
   # Connect network-output to the output process
   net.out_layer.out.connect(output_logger.a_in)

**Run the network**

.. code:: python

   from lava.magma import run_configs as rcfg
   from lava.magma import run_conditions as rcnd

   net.run(condition=rcnd.RunSteps(total_run_time), run_cfg=rcfg.Loihi1SimCfg())

Detailed Description
--------------------

.. toctree::
   :maxdepth: 1
   :caption: Detailed description:

   lava-lib-dl/slayer/slayer.rst
   lava-lib-dl/bootstrap/bootstrap.rst
   lava-lib-dl/netx/netx.rst