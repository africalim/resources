African Calibration and Imaging Infrastructure Setup
====================================================

The African Calibration and Imaging Workshop is hosted on an `Elastic Kubernetes Server <eks_>`_ (EKS) cluster.

Access to the cluster is provided through Github credentials.

If you don't yet have a Github account, please  `create one <github_join_>`_.

.. _join_group:

Join a Group
============

Workshop participants will be divided into groups of three or four and assigned
a group name based on the letters of the `Greek Alphabet <greek_alphabet_>`_,
such as alpha, beta, gamma, delta, lambda, mu, or nu.

Make a note of this name as your team will be assigned to a Kubernetes namespace
corresponding to this name.


Install kubectl
===============

Follow `these <install_kubectl_>`_ instructions for installing the
``kubectl`` command line tool, which will be used to authenticate with
the EKS cluster.


Obtain EKS cluster credentials
==============================

Navigate to the https://login.africalim.ratt.center login page and follow
the instructions which can by and large be copy-pasted into
your terminal and executed.

However, the ``set-context`` command should have ``--namespace=<group_name>``
appended to it, with ``<group_name>`` set to the group name assigned
`here <join_group_>`_:


.. code:: bash

  $ kubectl config set-context <context_name> \
    --cluster=<cluster_name> \
    --user=<user_name> \
    --namespace=<group_name>

This ensures that you access the section of the EKS cluster to which
you have been authenticated.


.. _eks: https://aws.amazon.com/eks/
.. _github_join: https://github.com/join
.. _greek_alphabet: https://en.wikipedia.org/wiki/Greek_alphabet
.. _login: https://login.africalim.ratt.center
.. _install_kubectl: https://kubernetes.io/docs/tasks/tools/#kubectl



