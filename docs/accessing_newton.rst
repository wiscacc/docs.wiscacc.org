================
Accessing Newton
================

You can access Newton via SSH from Euler. If you do not know how to access Euler using SSH, you
should read `Accessing Euler <http://docs.wiscacc.org/en/latest/accessing>`_ first. 

Before proceeding, you may not need to access newton directly! See 
`Using Newton <http://docs.wiscacc.org/en/latest/using_newton/#submitting-jobs>`_ to be sure. 

If you still feel that you need to access newton, follow the steps below.

1. Connect to Euler using SSH.
#. On Euler, run the command ``ssh newton``.
#. Enter your password (optionally, run ``ssh-copy-id euler`` to do this automatically).
#. Use Newton as necessary, then exit by typing ``exit`` or by pressing ``Control + D``.

Where is PBSWebmon?
-------------------

This question has popped up enough that it seems to warrant a response. If you would like to have 
a graphical representation of the currently working jobs on the cluster, you can access the 
PBSWebmon service for newton `here <http://euler.wacc.wisc.edu/cgi-bin/pbswebmon-newton.py>`_. 
Please note however, that due to some logistical concerns, this modified copy of the service does 
not provide many details about running jobs. If you would like to see lists of running jobs, use
the ``qstat [JobID]`` command.

Requesting Software
-------------------

If your project will not compile on Newton but normally would on Euler, there may be some 
packages not yet installed on the system. If you need something for this or some other reason, 
please contact the sysadmin to request that the package be installed. 


User Etiquette
--------------

The usage of Newton and its sub-cluster is governed by the same rules of etiquette as the usage 
of Euler. Likewise, it is important that large compilations not be run on Newton, and instead are 
run on one of its worker nodes. These can be accessed in the same manner as Newton or any other 
node by running ``ssh [nodename]`` from Euler.
