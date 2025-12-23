.. targets-start-do-not-remove

.. _Anaconda Documentation: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
.. _BOOST: https://www.boost.org/doc/libs/1_53_0/
.. _CMake: https://cmake.org/cmake/help/v3.14/
.. _CMake add_custom_target: https://cmake.org/cmake/help/latest/command/add_custom_target.html
.. _Doxygen: https://www.doxygen.nl/manual/docblocks.html
.. _Eigen: https://eigen.tuxfamily.org/dox/
.. _Sphinx: https://www.sphinx-doc.org/en/master/
.. _Breathe: https://breathe.readthedocs.io/en/latest/
.. _PEP-8: https://www.python.org/dev/peps/pep-0008/
.. _pipreqs: https://github.com/bndr/pipreqs 
.. _LaTeX: https://www.latex-project.org/help/documentation/
.. _W-13 DevOps Manual: https://xcp-confluence.lanl.gov/display/COM/W-13+DevOps
.. _upstream repository: https://re-git.lanl.gov/aea/material-models/tardigrade_micromorphic_tools
.. _Material Models: https://re-git.lanl.gov/aea/material-models
.. _UNIX group: https://ddw-confluence.lanl.gov/pages/viewpage.action?pageId=150929410
.. _`gersemi`: https://github.com/BlankSpruce/gersemi
.. _`clang-tidy`: https://clang.llvm.org/extra/clang-tidy/
.. _`clang-format`: https://clang.llvm.org/docs/ClangFormat.html

.. targets-end-do-not-remove

#############
solver\_tools
#############

*******************
Project Description
*******************

Tools for performing solves of nonlinear equations.

Information
===========

* Documentation: https://aea.re-pages.lanl.gov/material-models/tardigrade_solver_tools
* Wiki: https://re-git.lanl.gov/aea/material-models/tardigrade_solver_tools/-/wikis/home

Developers
==========

* Nathan Miller nathanm@lanl.gov
* Kyle Brindley kbrindley@lanl.gov

************
Dependencies
************

Compilers
=========

* c++11 compiler (listed version number has been tested at some point)

  * g++ >= GNU 4.8.5

Executables
===========

* [CMake](https://cmake.org/cmake/help/v3.14/) >= 3.14
* [Doxygen](https://www.doxygen.nl/manual/docblocks.html) >= 1.8.5
* [LaTeX](https://www.latex-project.org/help/documentation/) >= 2017

Python Modules (for documentation)
==================================

For convenience, the minimal Python environment requirements for the
documentation build are included in ``configuration_files/environment.yaml``.
This file was created from the [pipreqs](https://github.com/bndr/pipreqs)
command line tool and Sphinx configuration inspection, e.g. the extension
packages.

.. code-block:: bash

   $ pwd
   path/to/tardigrade_vector_tools/
   $ pipreqs --use-local --print --no-pin .

A minimal anaconda environment for building the documentation can be created
from an existing anaconda installation with the following commands.

.. code-block:: bash

   $ conda env create --file configuration_files/environment.yaml

You can learn more about Anaconda Python environment creation and management in
the [Anaconda
Documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

C++ Libraries
=============

.. note::

   **Non-admin installations for Eigen and Boost are no longer required.** This project is built and deployed against
   C++ libraries managed in Conda. See the Conda environment file and README discussion for non-admin environment
   management.

* [Eigen](https://eigen.tuxfamily.org/dox/) >= 3.3.7
* [BOOST](https://www.boost.org/doc/libs/1_53_0/) >= 1.53.0
* error\_tools: https://re-git.lanl.gov/aea/material-models/tardigrade_error_tools
* vector\_tools: https://re-git.lanl.gov/aea/material-models/tardigrade_vector_tools

If not found on the current system or active Conda environment, all of the
``*_tools`` libraries are pulled from their git repos by branch name and built
with their respective cmake files as part of the cmake build for this project.

**************
Build and Test
**************

This project is built with [CMake](https://cmake.org/cmake/help/v3.14/) and uses
[Sphinx](https://www.sphinx-doc.org/en/master/) to build the documentation with
[Doxygen](https://www.doxygen.nl/manual/docblocks.html) +
[Breathe](https://breathe.readthedocs.io/en/latest/) for the c++ API.

.. warning::

   **API Health Note**: The sphinx API docs are a work-in-progress. The doxygen
   API is much more useful

A build script has been created for convenience, ``new_build.sh``. It will build
everything including the library binary, the test binary, and the documentation.
This is the same build script used by ``jenkins_build.sh`` for CI builds and
testing.

sstelmo
=======

1) Activate a [W-13 Python Environment](https://xcp-confluence.lanl.gov/display/PYT/The+W-13+Python+3+environment)

   .. code-block:: bash

      $ module load python/2020.07-python-3.8
      $ sv3r

2) Build everything

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/

      # Just perform the build. Usage arguments are "cmake_build_type"
      ./new_build.sh None

      # Build and perform tests
      ./jenkins_build.sh

3) View test results

   .. code-block:: bash

      cat build/src/cpp/tests/results.tex

4) Display docs

   .. code-block:: bash

      # Sphinx
      firefox build/docs/sphinx/html/index.html &

      # Doxygen
      firefox build/docs/doxygen/html/index.html &

Local development
=================

In some cases it is not convenient to pull down every repository required but it may be desired that local
versions of the repository are used. An example of when this may be needed is if development is across
multiple libraries and is proceeding faster than collaborators can check in results. In this case, and
outside of developers no-one should need to do this, a version of the code using local repositories can be
built.

1) Activate a [W-13 Python Environment](https://xcp-confluence.lanl.gov/display/PYT/The+W-13+Python+3+environment)

   .. code-block:: bash

      $ module load python/2020.07-python-3.8
      $ sv3r

2) Define convenience environment variables

   .. code-block:: bash

      $ my_tardigrade_error_tools=/path/to/my/tardigrade_error_tools
      $ my_tardigrade_vector_tools=/path/to/my/tardigrade_vector_tools

3) Perform the initial configuration

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools
      $ mkdir build
      $ cd build
      $ cmake3 .. -DFETCH_SOURCE=LOCAL -DTARDIGRADE_ERROR_TOOLS_PATH=${my_tardigrade_error_tools} -DTARDIGRADE_VECTOR_TOOLS_PATH=${my_tardigrade_vector_tools}

4) Building the library

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/build
      $ make

Building the documentation
==========================

To build just the documentation pick up the steps here:

2) Create the build directory and move there

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/
      $ mkdir build/
      $ cd build/

3) Run cmake3 configuration

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/build/
      $ cmake3 ..

4) Build the docs

   .. code-block:: bash

      $ cmake3 --build docs

5) Documentation builds to:

   .. code-block:: bash

      tardigrade_solver_tools/build/docs/sphinx/html/index.html

6) Display docs

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/build/
      $ firefox docs/sphinx/html/index.html &

7) While the Sphinx API is still a WIP, try the doxygen API

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/build/
      $ firefox docs/doxygen/html/index.html &

*******************
Install the library
*******************

Build the entire before performing the installation.

4) Build the entire project

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/build
      $ cmake3 --build .

5) Install the library

   .. code-block:: bash

      $ pwd
      /path/to/tardigrade_solver_tools/build
      $ cmake --install . --prefix path/to/root/install

      # Example local user (non-admin) Linux install
      $ cmake --install . --prefix /home/$USER/.local

      # Example install to conda environment
      $ conda active my_env
      $ cmake --install . --prefix ${CONDA_DEFAULT_ENV}

      # Example install to W-13 CI/CD conda environment performed by CI/CD institutional account
      $ cmake --install . --prefix /projects/aea_compute/release

***********************
Contribution Guidelines
***********************

.. contribution-start-do-not-remove

Git Commit Message
==================

Begin Git commit messages with one of the following headings:

* BUG: bug fix
* DOC: documentation
* FEAT: feature
* MAINT: maintenance
* TST: tests
* REL: release
* WIP: work-in-progress

For example:

.. code-block:: bash

   git commit -m "DOC: adds documentation for feature"

Git Branch Names
================

When creating branches use one of the following naming conventions. When in
doubt use ``feature/<description>``.

* ``bugfix/\<description>``
* ``feature/\<description>``
* ``release/\<description>``

reStructured Text
=================

[Sphinx](https://www.sphinx-doc.org/en/master/) reads in docstrings and other special portions of the code as
reStructured text. Developers should follow styles in this [Sphinx style
guide](https://documentation-style-guide-sphinx.readthedocs.io/en/latest/style-guide.html#).

Style Guide
===========

This project uses the `gersemi`_ CMake linter. The CI style guide check runs the following command

.. code-block:

   $ gersemi CMakeLists.txt src/ docs/ --check

and any automatic fixes may be reviewed and then applied by developers with the following commands

.. code-block:

   $ gersemi CMakeLists.txt src/ docs/ --diff
   $ gersemi CMakeLists.txt src/ docs/ --in-place

This project enforces its style using `clang-tidy`_ and `clang-format`_ as configured with the
`.clang-format` and `.clang-tidy` files in the root directory. The formatting of the project can be
checked using `clang-tidy`_ by first configuring the project using

.. code-block:

   $ cmake -S . -B build ... -DCMAKE_EXPORT_COMPILE_COMMANDS=ON

where `...` are the other configuration flags specified. After this clang-tidy can be run on the
full project from the source directory via

.. CAUTION::
    Commit all changes prior to running the clang tidy command. This will edit all source files.

.. code-block:

   $ run-clang-tidy -config-file=.clang-tidy -header-filter=*.h -p build

The formatting can be checked using `clang-format`_ by running

.. code-block:

   $ cmake -S . -B build ...
   $ cmake --build build --target cpp-format-check

which will indicate if the formatting is correct. The c++ files can be re-formatted to match the
style guidance by running

.. CAUTION::
    Commit all changes prior to running the format command. This will edit all source files.

.. code-block

   $ cmake --build build --target cpp-format

If the style is not constrained by the above, it should be inferred by the surrounding code.
Wherever a style can't be inferred from surrounding code this project falls back to `PEP-8`_-like
styles the exceptions to the notional PEP-8 fall back:

1. `Doxygen`_ style docstrings are required for automated, API from source documentation.

.. contribution-end-do-not-remove
