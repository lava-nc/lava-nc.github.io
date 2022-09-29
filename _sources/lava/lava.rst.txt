Lava
====

This page gives an overview over all public Lava modules, classes and functions. 

Lava is divided into the sub-packages:

- :ref:`The process library <lava/lava.proc:Lava process library>` containing commonly used :ref:`Processes <lava/lava.magma.core.process:lava.magma.core.process.process>` and :ref:`ProcessModels <lava/lava.magma.core.model:lava.magma.core.model.model>`.
- :ref:`Magma <lava/lava.magma:Magma>`, containing the main components of Lava:
  
  - :ref:`Magma core <lava/lava.magma.core:lava.magma.core>`
    base classes, definitions and functionality 
  - :ref:`Magma compiler <lava/lava.magma.compiler:lava.magma.compiler>`
    compiling and building the network and communication channels
  - :ref:`Magma runtime <lava/lava.magma.runtime:lava.magma.runtime>`
    providing a frontend for execution and control

Lava's fundamental concepts and key components are described in :ref:`Lava Architecture <lava architecture>`.

Explanatory tutorials and example code can be found in the :ref:`in-depth tutorials <getting_started/in_depth_tutorials:fundamental concepts>` and in the :ref:`End-to-end Tutorial notebooks <getting_started/end_to_end_tutorials:application examples>`. 



.. py:module:: lava


.. toctree::
   :maxdepth: 2

   lava.magma
   lava.proc
   lava.utils
