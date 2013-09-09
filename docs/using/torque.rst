==========================
Submitting Jobs via TORQUE
==========================

The TORQUE Resource Manager is used for both resource management and job scheduling. Users wishing to run code on Euler must submit a job to TORQUE, which will then automatically send the job to a compute node which meets the job's requirements. The following provides only a cursory overview of how to submit and monitor jobs. Full documentation is available from `Adaptive Computing <http://www.adaptivecomputing.com/resources/docs/torque/4-0-2/help.htm#topics/2-jobs/jobSubmission.htm>`_.

--------
Overview
--------
Jobs submitted to TORQUE are configured by creating a shell script with special TORQUE/PBS-specific configuration comments. An example of these scripts is provided in your home directory, under the sym-linked directory `Example Jobs`. A few of these examples will be covered later on.

Each job is assigned on ID of the form ``n.euler`` where n is an incrementally-increasing integer. This ID is used to monitor job status, delete jobs, and to organize output and error logs.

``qsub``: Job Submission
------------------------
All configuration options may be set in either the job submission script or on the command line. The most basic job can be submitted by ``qsub script.sh``, where ``script.sh`` is the name of your job submission script. One useful argument is: ``-l nodes=n,ppn=x,gpus=y`` to reserve `n` nodes and `y` GPUs per node, each with `x` processors (`ppn`: processors per node). Another useful argument is for tasks: ``-t x-y,z`` which will run the job `y-x+1` times, each with a different number from the set of the range `x-y`, and `z`. One place where this would be useful would be for selectively rendering frames of an animation.

``qstat``: Job Status
---------------------
Job status may be monitored via ``qstat``. Without any arguments, this will list all recent jobs that have submitted. Next to each job ID will be a single character for the status. 'Q' for queued, 'R' for running, 'E' for error, and 'C' for completed. qstat will also show how long the job has run and how much time allotted remains. Depending on your submission settings, you will also receive an email saying whether your job completed or failed, and why. By default this is sent to your account on the cluster itself, which is accessible by the command mail.

``qdel``: Job Removal
---------------------
Jobs can be removed or cancelled via the command ``qdel id`` where `id` is the job's ID number.

``pbsnodes`` and ``pbstop``: Node Monitoring
--------------------------------------------
The command ``pbsnodes -a`` will give you details about all nodes in the cluster (available/used memory, number of jobs running on the node, various stats on the node's GPUs). The command ``pbstop`` will give you a nicer view of this data.

--------------------------
Job Submission Walkthrough
--------------------------
This section describes the steps required to log in to Euler and submit a basic GPU job.

A Basic Job
-----------
As seen above, your home directory contains a symlink to the folder `Example Jobs`. This folder contains some example TORQUE job submission scripts. To run one of these, copy the ``gpu-scan.sh`` script to your home directory and submit it to the job manager using ``qsub``.

::

    #!/bin/sh
    #PBS -N gpu-scan
    #PBS -l nodes=1:gpus=1,walltime=00:01:00
    
    $NVSDKCOMPUTEROOT/bin/linux/release/scan

The script above has two main sections: job settings and the script itself. Lines 3 and 4 set the job’s name to `gpu-scan` (``-N gpu-scan``) and requests one node (``nodes=1``) with one GPU (``gpus=1``). Additionally, it tells the scheduler that the job will take at most 1 minute to execute (``walltime=00:01:00``). The last line executes the ``scan`` code example from the CUDA SDK.

There are three main things to note in this example. First, job settings are specified by ``#PBS`` followed by command line options for ``qsub``. Since the settings are defined in comments, you can usually directly run this script from the command line without the TORQUE-specific options causing any issues. Next, all settings defined in the script can also be passed directly to ``qsub`` from the command line. See the ``qsub`` man page for more options (``man qsub``). Lastly, note the use of the ``$NVSDKCOMPUTEROOT`` or ``$CUDA_SDK`` environment variables. This allows the same script to be used with differing versions of the CUDA SDKs, depending on which version you have selected via ``module load cuda``.

:: 

    [aseidl@euler ~]$ cp Example\ Jobs/gpu-scan.sh .
    [aseidl@euler ~]$ qsub gpu-scan.sh
    8117.euler
    [aseidl@euler ~]$ qstat 8117
    Job id        Name         User        Time Use S Queue
    ------------- ------------ ----------- -------- - -----
    8117.euler    gpu-scan     aseidl      00:00:01 C batch

The example above shows how to copy the example job script to your home directory, submit it to the TORQUE job scheduler, and check on the status of the submitted job. When submitting the job, ``qsub`` will output the job ID assigned to it. This ID can then be use to check on the job’s status with ``qstat``. You can also enter ``qstat`` with no arguments to see the status of all recently submitted jobs.

The output of ``qstat`` shows the job ID assigned, the name given via the ``-N`` option, the user who submitted the job, walltime used, job status, and queue which the job was submitted to. Walltime is defined as the actual time that the job ran. Status is a single letter, the most common of which are: Q (queued), R (running), and C (completed). The queue allows you to submit your job to different sections of the cluster which may have different resources available or different restrictions on parameters such as walltime, number of GPUs requested, etc. Euler currently has a single queue, called ‘batch’.

Interactive Jobs
----------------
To run a job manually on a compute node, you can submit what is called an "interactive job" to TORQUE. This is done via the command ``qsub -I``. Once the appropriate resources are available, you will be presented with a shell on one of the compute nodes, as shown below.

::

    [aseidl@euler ~]$ qsub -I
    qsub: waiting for job 8123.euler to start
    qsub: job 8123.euler ready
    
    [aseidl@euler01 ~]$

Note that unlike non-interactive jobs, interactive ones (and any processes your started during the interactive job) will terminate as soon as you exit the node or log out of Euler.
