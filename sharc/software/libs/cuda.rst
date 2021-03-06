.. _cuda_sharc:

CUDA
====

CUDA, which stands for Compute Unified Device Architecture, is a parallel computing platform and application programming interface (API) model created by NVIDIA.
It allows software developers to use a CUDA-enabled graphics processing unit (GPU) for general purpose processing - an approach known as *General Purpose GPU* (GPGPU).

Usage
-----

There are several versions of the CUDA library available. As with many libraries installed on the system, CUDA libraries are made available via ``module`` commands which are only available once you have started a ``qrshx`` session with a GPU resource ``-l gpu=``. For example, to request a single GPU: ::

  qrshx -l gpu=1

**See** :ref:`GPUComputing_sharc` **for more information on how to request a GPU-enabled node for an interactive session or job submission.** 

Load a specific version with one of the following: ::

        module load libs/CUDA/9.1.85/binary
        module load libs/CUDA/8.0.44/binary
        module load libs/CUDA/7.5.18/binary

To check which version of CUDA you are using ::

        $ nvcc --version
        nvcc: NVIDIA (R) Cuda compiler driver
        Copyright (c) 2005-2017 NVIDIA Corporation
        Built on Fri_Nov__3_21:07:56_CDT_2017
        Cuda compilation tools, release 9.1, V9.1.85

**Important** To compile CUDA programs you also need a compatible version of the :ref:`gcc_sharc`.  As of version 8.0.44, CUDA is compatible with GCC versions:

* greater than or equal to 4.7.0 (to allow for the use of c++11 features) and
* less than 5.0.0

It is therefore recommended that you load the most recent 4.x version of GCC when building CUDA programs: ::

        module load dev/gcc/4.9.4

Compiling the sample programs
-----------------------------
You do not need to be using a GPU-enabled node to compile the sample programs but you do need a GPU to run them.

In a ``qrsh`` session ::

        # Load modules
        module load libs/CUDA/9.1.85/binary
        module load dev/gcc/4.9.4

        # Copy CUDA samples to a local directory
        # It will create a directory called NVIDIA_CUDA-9.1_Samples/
        mkdir cuda_samples
        cd cuda_samples
        cp -r $CUDA_SDK .

        # Compile (this will take a while)
        cd NVIDIA_CUDA-9.1_Samples/
        make

A basic test is to run one of the resulting binaries, ``deviceQuery``.

Documentation
-------------
* `CUDA Toolkit Documentation <http://docs.nvidia.com/cuda/index.html#axzz3uLoSltnh>`_
* `The power of C++11 in CUDA 7 <http://devblogs.nvidia.com/parallelforall/power-cpp11-cuda-7/>`_

Profiling using nvprof
----------------------

Note that ``nvprof``, NVIDIA's CUDA profiler cannot write output to the ``/fastdata`` filesystem.

This is because the profiler's output is a `SQLite <https://www.sqlite.org/>`__ database and SQLite requires a filesystem that supports file locking
but file locking is not enabled on the (`Lustre <http://lustre.org/>`__) filesystem mounted on ``/fastdata`` (for performance reasons). 

CUDA Training
-------------

`GPUComputing@sheffield <http://gpucomputing.shef.ac.uk>`_ provides a self-paced `introduction to CUDA <http://gpucomputing.shef.ac.uk/education/cuda/>`_ training course.

Determining the NVIDIA Driver version
-------------------------------------
Run the command: ::

        cat /proc/driver/nvidia/version

Example output is ::

        NVRM version: NVIDIA UNIX x86_64 Kernel Module  390.30  Wed Jan 31 22:08:49 PST 2018
        GCC version:  gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC) 

Installation notes
------------------
These are primarily for system administrators.

Device driver
^^^^^^^^^^^^^

The NVIDIA device driver is installed and configured using the ``gpu-nvidia-driver`` systemd service (managed by puppet).
This service runs ``/usr/local/scripts/gpu-nvidia-driver.sh`` at boot time to:

- Check the device driver version and uninstall it then reinstall the target version if required;
- Load the ``nvidia`` kernel module;
- Create several *device nodes* in ``/dev/``.

The NVIDIA device driver is currently version 390.30.

CUDA 9.1.85
^^^^^^^^^^^

#. Installed with :download:`install.sh </sharc/software/install_scripts/libs/CUDA/install.sh>` with ``9.1.85_387.26`` as the sole argument. 
   This installs the toolkit and three NVIDIA-recommended patches.
#. :download:`Modulefile </sharc/software/modulefiles/libs/CUDA/9.1.85/binary>` was installed as ``/usr/local/modulefiles/libs/CUDA/9.1.85/binary``

CUDA 8.0.44
^^^^^^^^^^^

#. Installed with :download:`install.sh </sharc/software/install_scripts/libs/CUDA/install.sh>` with ``8.0.44`` as the sole argument.
#. :download:`This modulefile </sharc/software/modulefiles/libs/CUDA/8.0.44/binary>` was installed as ``/usr/local/modulefiles/libs/CUDA/8.0.44/binary``

CUDA 7.5.18
^^^^^^^^^^^

#. Installed with :download:`install.sh </sharc/software/install_scripts/libs/CUDA/install.sh>` with ``7.5.18`` as the sole argument.
#. :download:`This modulefile </sharc/software/modulefiles/libs/CUDA/7.5.18/binary>` was installed as ``/usr/local/modulefiles/libs/CUDA/7.5.18/binary``
