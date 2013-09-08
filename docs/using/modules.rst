===================
Environment Modules
===================

Euler uses `Environment Modules <http://modules.sourceforge.net>`_ to provide access to the various software packages available to users. These software packages include various versions of:

- `Blender <http://blender.org>`_
- `CUDA <http://nvidia.com/cuda>`_
- MVAPICH2
- OpenMPI
- `Paraview <http://paraview.org>`_
- `Point Cloud Library <http://pointclouds.org>`_

Running ``module avail`` will print a list of all modules available to you. See the `Environment Modules documentation <http://modules.sourceforge.net/man/module.html>`_ or ``man module`` for a list of all sub-commands. The main ones to pay attention to are:

``module avail``
    List modules available to you

``module load pkg/vers``
    Load the package given in `pkg/vers`

``module unload pkg/vers``
    Unload the package

``module initadd pkg/vers``
    Set `pkg/vers` to be loaded automatically at login

