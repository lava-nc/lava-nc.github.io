.. raw:: html

   <style> .bug {color:#c9838c}
           .docs {color:#35abff}
           .enhance {color:#72adad}
           .reva {color:#0e9e15}
           .revd {color:#5a6975}
           .revn {color:#9fab54}
           .needsr {color:#f78864}
           .duplicate {color:#7e8185}
           .invalid {color:#adad4b}
           .wontfix {color:#707070}
    </style>
.. role:: bug
.. role:: docs
.. role:: enhance
.. role:: reva
.. role:: revd
.. role:: revn
.. role:: needsr
.. role:: duplicate
.. role:: invalid
.. role:: wontfix

Developer Guide
###############
Welcome to the Lava Developers Guide! Lava is an open-source software framework and community for developing neuro-inspired applications and mapping them to neuromorphic hardware.

Lava is an open, community-developed code base. 

The purpose of this document is to provide a guide for contributing to Lava and explain how the Lava Project operates. The Lava Project can only grow through the contributions and work of this community. 

Thanks for your interest in contributing to Lava!

Lava's Origins
**************
The initial code for the Lava project (`github.com/lava-nc <https://github.com/lava-nc>`_) was seeded by Intel's Neuromorphic Computing Lab (NCL), a group within Intel Labs. NCL researchers continue to actively manage and expand the functionality of Lava, with the goal of building an active community of contributors and users.

Contact Information
*******************
To receive regular updates on the latest developments and releases of the Lava Software Framework please `subscribe to our newsletter <http://eepurl.com/hJCyhb>`_.

Email the Intel Lava Team at: lava@intel.com

Table of Contents
*****************

- :ref:`Development Roadmap`
- :ref:`How to contribute to Lava`
- :ref:`Coding Conventions`
- :ref:`Contributors`
- :ref:`Repository Structure`
- :ref:`Code of Conduct`

Development Roadmap
*******************
Our development roadmap will be published soon.

Initial Release
===============

+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Component                          | HW support   | Features                                                                                                                                                                                                                     |
+====================================+==============+==============================================================================================================================================================================================================================+
| Magma                              | CPU, GPU     | - The generic high-level and HW-agnostic API supports creation of processes that execute asynchronously, in parallel and communicate via messages over channels to enable algorithm and application development.             |
|                                    |              | - Compiler and Runtime initially only support execution or simulation on CPU and GPU platform.                                                                                                                               |
|                                    |              | - A series of basic examples and tutorials explain Lava's key architectural and usage concepts                                                                                                                               |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Process library                    | CPU, GPU     | Initially supports basic processes to create spiking neural networks with different neuron models, connection topologies and input/output processes.                                                                         |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deep Learning library              | CPU, GPU     | Allows for direct training of stateful and event-based spiking neural networks with backpropagation via SLAYER 2.0 as well as inference through Lava. Training and inference will initially only be supported on CPU/GPU HW. |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Optimization library               | CPU, GPU     | Offers a variety of constraint optimization solvers such as constraint satisfaction (CSP) or quadratic unconstraint binary optimization (QUBO) and more.                                                                     |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dynamic Neural Field library       | CPU, GPU     | Allows to build neural attractor networks for working memory, decision making, basic neuronal representations, and learning.                                                                                                 |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Magma and Process library          | Loihi 1, 2   | Compiler, Runtime and the process library will be upgraded to support Loihi 1 and 2 architectures.                                                                                                                           |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Profiler                           | CPU, GPU     | Enables power and performance measurements on neuromorphic HW as well as the ability to simulate power and performance of neuromorphic HW on CPU/GPU platforms. Initially only CPU/GPU support will be available.            |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| DL, DNF and Optimization library   | Loihi 1, 2   | All algorithm libraries will be upgraded to support and be properly tested on neuromorphic HW.                                                                                                                               |
+------------------------------------+--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

How to contribute to Lava
*************************
Contributions to Lava are made through pull requests from personal forks of Lava (https://github.com/lava-nc) on Github.
We welcome contributions at all levels of the software stack:

- Runtime
- Compiler
- API
- New Processes
- Algorithm or application libraries built on top of Lava
- Seperate utilities
- 3rd party interfaces

Before you submit a pull request, please create an `issue <https://github.com/lava-nc/lava/issues>`_ that describes why the pull request is needed.
Please link your pull request to the issue covering the request upon pull request creation.

Open an Issue
=============
If you find a bug or would like to add additional functionality to Lava follow the steps below to create an issue.

.. note::
   These instructions are written using lava-nc/lava repository, however they can be used with any of the lava-nc/<repo> repositories.

- Open an `issue <https://github.com/lava-nc/lava/issues>`_
- Add a descriptive title
- Describe the issue in detail in the body
- Add an issue type `label <https://github.com/lava-nc/lava/labels>`_:
  
  - :docs:`documentation`
  - :enhance:`enhancement`
  - :bug:`bug`

- Add a review `label <https://github.com/lava-nc/lava/labels>`_:
  
  - :needsr:`needs-review`

- The issue will be reviewed by a lava committer

  - Please participate in the review
  - Respond to any questions
  - Update the issue with changes as requested

- The issue will be triaged with labels based on status:

  - :reva:`reviewed-approved`
  - :revd:`reviewed-declined`
  - :revn:`reviewed-needs-work`
  - :duplicate:`duplicate`
  - :invalid:`invalid`
  - :wontfix:`wontfix`
  - <release version>-target

- If 'reviewed-approved' a label of '<release version>-target' will be added

Pull Request Checklist
======================
Before you send your pull requests follow these steps:

- Read the :ref:`Code of Conduct`
- Check if your changes are consistent with :ref:`Coding Conventions`
- :ref:`Apply a license<Add a License>` to your contributions
- Run :ref:`linting and unit tests<Lint Unit Tests>`

.. warning::
   Code submissions must be original source code written by you.

Open a Pull Request
===================
For full coverage of how to create a fork and work with it see `Github Fork Procedures <https://docs.github.com/en/github/collaborating-with-pull-requests>`_

.. note::
   These instructions are written using lava-nc/lava repository, however they can be used with any of the lava-nc/<repo> repositories.

- Fork `lava-nc/lava <https://github.com/lava-nc/lava>`_
  
  - Click on the 'Fork' button in the upper right corner 

- Get the code locally
  
  - Clone your fork to your local machine
    
    .. code-block:: bash

       git clone git@github.com:<user-name>/lava.git

  - Alternatively add a remote from your local repository to your fork
    
    .. code-block:: bash
    
       git remote add lava-fork git@github.com:<user-name>/lava.git

- Create a new descriptive branch

  .. code-block:: bash

     git checkout -b <branch-name>

- Write your code
  
  - Make code changes
  - Run linting and unit tests

    .. _Lint Unit Tests:

    .. code-block:: bash
    
       # Install poetry
       pip install "poetry>=1.1.13"
       poetry config virtualenvs.in-project true
       poetry install
       poetry shell

       # Run linting
       flakeheaven lint src/lava tests

       # Run unit tests
       pytest

       # Run Secuity Linting
       bandit -r src/lava/.

       #### If security linting fails run bandit directly
       #### and format failures
       bandit -r src/lava/. --format custom --msg-template '{abspath}:{line}: {test_id}[bandit]: {severity}: {msg}'

  - Fix any issues flagged by linting and unit tests and check again to ensure the issues are resolved

    .. _Add a License:

    .. note::

       Please include, at the top of each source file, a BSD 3 or LGPL 2.1+ License. Check with Lava Committers if you have a question about licenses.

       For Lava code contributions *excepting* **lava-nc/magma/compiler** and **lava-nc/magma/runtime**, use BSD 3. Example Intel BSD 3 License:

       | # Copyright (C) 2021 Intel Corporation
       | # SPDX-License-Identifier: BSD-3-Clause
       | # See: https://spdx.org/licenses/

       For **lava-nc/magma/compiler** and **lava-nc/magma/runtime** use either BSD 3 or LGPL 2.1+. Example Intel LGPL 2.1+ License:

       | # Copyright (C) 2021 Intel Corporation
       | # SPDX-License-Identifier: LGPL-2.1-or-later
       | # See: https://spdx.org/licenses/

  - Commit code changes to your branch

    .. code-block:: bash
    
       git add <code>
       # Sign your commit and add a commit summary
       git commit -sm "<title-description>"

- Push this branch to your fork

    .. code-block:: bash

       # If you cloned
       git push -u origin <branch-name>
       # If you added remote lava-fork
       git push -u lava-fork <branch-name>
       

- `Open a pull request <https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork>`_ to `lava-nc/lava main <https://github.com/lava-nc/lava/tree/main>`_
   
  - Add a descriptive pull request title
  - Describe the pull request in detail in the body
  - Link an issue to the pull request
  - Add a pull request label:

  - :docs:`documentation`
  - :enhance:`enhancement`
  - :bug:`bug`

- Add a review `label <https://github.com/lava-nc/lava/labels>`_:
  
  - :needsr:`needs-review`

- Work with committers on your code review
  
  - Answer any questions or comments
  - Make updates as required
  - Meet code requirements before merge

    |        - Unit tests cover the pull request
    |        - Code contains class and method doc strings
    |        - The build and tests on the pull request pass
    |        - Style Guidelines are met
- Required before merge: 2 Code Reviews and the approval of a committer

Coding Conventions
******************

Code Requirements
=================
- Code must be **styled** according to `PEP8 <https://www.python.org/dev/peps/pep-0008/>`_.

  - Line limit is 80 characters
  - Use short descriptive variable & function names

- Code must use **docstrings**:

  - module docstring
  - class docstring
  - method docstring
  - See :ref:`docstring format<Docstring Format>`

- Code should be developed using TDD (Test Driven Development)

  - Descriptive **unit tests** are *required*

    - Tests prove your code works
    - Tests help keep your code working when other contribute
    - Write descriptive unit tests that explain the intent of the test
    - Write minimal unit tests for just the feature you want to test

  - Unit tests must cover the code in each pull request

- All continuous integration checks must **pass** before pull requests are merged
- Code must be reviewed twice and merged by a :ref:`committer`

Guidelines
==========
- Before you embark on a big coding project, document it with an :ref:`issue <Open an Issue>` and discuss it with others including Lava Committers.
- Use consistent :ref:`numpy docstring format<Docstring Format>`
- Strive for a 100% linter score
- Prefer short yet descriptive variable and function names
- The more global a variable or function name the longer it may be. The more local the shorter the name should be
- When something breaks, many tests may fail. But don't be overwhelmed. Fix the lowest level unit tests first. Chances are good these will also fix higher level unit tests.
- Document everything

  - module doc string
  - class doc string
  - method docstring

Docstring Format
===============

.. code-block:: python

   # Use numpy-style docstring formatting: https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard
   def function(self, arg1, arg2):
   """ <Short description>
   <Optional: Detailed description>

   Parameters
   ----------
   arg1
   arg2

   Returns
   -------

   """

Contributors
************

Contributor
===========
A contributor is someone who contributes to the Lava Community. Contributions can take many forms and include:

- Create and update documentation
- Contribute code
- Contribute reviews of code and issues
- Rate, comment on and give "thumbs up" on issues, pull requests etc.

Committer
=========
A Committer is a contributor who has the authority, and responsibility to review code and merge pull requests in the Lava Community. Committers are the leaders of the Lava Project and they have the following responsibilities:

- Plan and decide the direction of the Lava Project
- Facilitate community meetings and communication
- Mentor new developers and community members
- Review issues
- Review pull requests, enforce requirements, merge code
- Keep CI working

List of lava-nc/lava Project Committers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- `awintel <https://github.com/awintel>`_
- `joyeshmishra <https://github.com/joyeshmishra>`_
- `ysingh7 <https://github.com/ysingh7>`_
- `jlakness-intel <https://github.com/jlakness-intel>`_
- `mgkwill <https://github.com/mgkwill>`_

List of lava-nc/lava-dnf Project Committers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- `mathisrichter <https://github.com/mathisrichter>`_
- `awintel <https://github.com/awintel>`_
- `joyeshmishra <https://github.com/joyeshmishra>`_
- `ysingh7 <https://github.com/ysingh7>`_
- `jlakness-intel <https://github.com/jlakness-intel>`_
- `mgkwill <https://github.com/mgkwill>`_

List of lava-nc/lava-optimization Project Committers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- `GaboFGuerra <https://github.com/GaboFGuerra>`_
- `awintel <https://github.com/awintel>`_
- `joyeshmishra <https://github.com/joyeshmishra>`_
- `ysingh7 <https://github.com/ysingh7>`_
- `jlakness-intel <https://github.com/jlakness-intel>`_
- `mgkwill <https://github.com/mgkwill>`_

List of lava-nc/lava-dl Project Committers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- `srrisbud <https://github.com/srrisbud>`_
- `bamsumit <https://github.com/bamsumit>`_
- `awintel <https://github.com/awintel>`_
- `joyeshmishra <https://github.com/joyeshmishra>`_
- `ysingh7 <https://github.com/ysingh7>`_
- `jlakness-intel <https://github.com/jlakness-intel>`_
- `mgkwill <https://github.com/mgkwill>`_


Committer Promotion
~~~~~~~~~~~~~~~~~~~
Committer promotion is the responsibility of the current committers.

A current committer can nominate a contributor to become a committer based on contributions to the Lava Community.

Contributions that qualify a contributor should include:

- Contributing significant document creation and document updates
- Contributing significant amounts of code and high value features
- Contributing reviews of code and issues
- Facilitation of community planning, communication, and infrastructure

Upon nomination of a new committer, all current committers will vote on the new committer nomination.

- A quorum of 80% of committers must vote for a valid election
- A nominee must not receive any vetos

Repository Structure
********************
Lava directory structure:

| lava/
| ├── lava
| ├── lava-dl
| ├── lava-dnf
| ├── lava-docs
| └── lava-optimization

lava-nc/lava
============
.. epigraph::
   Core repository containing the runtime, compiler and API.

lava-nc/lava-dnf
================
.. epigraph::
   Lava Dynamic Neural Fields Library

lava-nc/lava-dl
===============
.. epigraph::
   Lava Deep Learning Library

lava-nc/lava-optimization
=========================
.. epigraph::
   Lava Neuromorphic Constraint Optimization Library

lava-nc/lava-docs
=================
.. epigraph::
   Lava Documentation


Code of Conduct
***************
Your behavior contributes to a successful community. As such community members should observe the following behaviors:

- Welcome others, use inclusive and positive language.
- Be truthful and transparent.
- Be respectful of difference in viewpoint and experience.
- Work towards the community's best interest.
- Show empathy towards others.
- Accept constructive criticism.

All Lava spaces are **professional interaction spaces** and *prohibit inappropriate behavior* or any behavior that could reasonably be thought to be inappropriate.

Inappropriate behavior that is **intolerable** includes:

- Harassment in any form.
- Sexual language or images.
- Without permission, sharing private information of another, i.e., electronic, or physical address.
- Political attacks, derogatory or insulting comments.
- Conduct which could reasonably be considered inappropriate for the forum in which it occurs.

Licenses
********
Lava is licensed as *BSD 3* or *LGPL 2.1+*. Specific components are licensed as follows:

| lava-nc/lava/magma/core:            **BSD 3-Clause**
| lava-nc/lava/magma/compiler:        **LGPL 2.1 or later**
| lava-nc/lava/magma/runtime:         **LGPL 2.1 or later**
|
| lava-nc/lava/proc:                  **BSD 3-Clause**
| lava-nc/lava/utils:                 **BSD 3-Clause**
| lava-nc/lava/tutorials:             **BSD 3-Clause**
|
| lava-nc/lava-dl:                    **BSD 3-Clause**
| lava-nc/lava-dnf:                   **BSD 3-Clause**
| lava-nc/lava-optimization:          **BSD 3-Clause**

Go to :ref:`how to apply a license<Add a License>` for more information on using a license in your contribution.
