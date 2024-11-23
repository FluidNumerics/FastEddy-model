.. _run_fasteddy_amdgpu:

**************************
Running under NSF NCAR HPC
**************************

These instructions will help users get started running FastEddy on systems with AMD GPUs.

Prerequisites
=============
Before attempting to build FastEddy on a platform with AMD GPUs, verify that you have the following dependencies on your system

* AMD Instinct GPU 
* `ROCm 6.2 or later <https://rocm.docs.amd.com/projects/install-on-linux/en/latest/>`_
* `GPU Aware MPI installation <https://rocm.blogs.amd.com/software-tools-optimization/gpu-aware-mpi/README.html>`_run_fasteddy
* NetCDF-C library


Compilation
===========

The Makefile-based build system included here assumes deployment on the NSF
NCAR HPCs. FastEddy requires a C-compiler, MPI, and CUDA. Currently, the
default modules loaded at login suffice on Casper, however the :code:`cuda` module
will need to be loaded on Derecho by running :code:`module load cuda`.

   1. Download the source code from the `Releases <https://github.com/NCAR/FastEddy-model/releases>`_ page and unpack the release in the desired location or clone the `repository <https://github.com/NCAR/FastEddy-model>`_ in the desired location.

   2. Navigate to the **SRC/FEMAIN** directory.

   3. To build the FastEddy executable run :code:`make -f Makefile.hip` (optionally run :code:`make clean` first if appropriate). Note that you may need to define the `OTHER_INCLUDES` environment variable to define the includes directories for NetCDF and OpenMPI, e.g. :code:`export OTHER_INCLUDES="-I${NETCDF_C_ROOT}/include -I${OPENMPI_ROOT}/include"`.

The :code:`Makefile.hip` will convert all CUDA API calls and :code:`#include` headers to the appropriate HIP API calls and headers using the  :code:`hipify-perl -inplace` command.
Following this, the code is compiled similarly to the CUDA version, but using the HIP compiler and libraries.

The :code:`FastEddy` executable will be located in the **SRC/FEMAIN** directory. To
build on other HPC systems with NVIDIA GPUs, check for availability of the aformentioned
modules/dependencies. Successful compilation may require modifications to shell environment
variable include or library paths, or alternatively minor adjustments to the include or library
flags in **SRC/FEMAIN/Makefile**.
