Africalim Infrastructure Setup
====================================================

The Africalim Workshop is hosted on an `Elastic Kubernetes Server <eks_>`_ (EKS) cluster.

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

Test EKS cluster credentials
============================

Once credentials have been obtained, the following command and response
indicates that authentication and communication with the EKS cluster
has been established:

.. code:: bash

  $ kubectl get pods
  No resources found in <group_name> namespace.

If this does not work without appending your namespace to the command then run

.. code:: bash

  $ kubectl config set-context --current --namespace=<group_name>

Using radiopadre to access EFS
===============================

Create a fresh virtual environment and install radiopadre-client using

.. code:: bash

  pip install git+https://github.com/ratt-ru/radiopadre-client.git@b1.2.3 kubernetes

Then you should be able to run

.. code:: bash

  run-radiopadre -K <group-name>-efs-pvc: --k8s-node-selector rarg/node-class=compute,rarg/instance-type=m5.4xlarge -e --k8s-uid 1000 --k8s-gid 1000

This will bring up the radiopadre server in a pod which opens a file browser and a CARTA instance.
We'll use CARTA to inspect fits files later on. For now simply make sure you can access the file your hello-world script writes to.


Running the KGB recipe
=======================

Install the following version of cult-cargo (possibly in the fresh virtual environment)

.. code:: bash

  pip install git+https://github.com/caracal-pipeline/cult-cargo.git@pfb-imaging kubernetes

Then run the KGB recipe using

.. code:: bash

  stimela -C run recipes/luno_kgb.yaml recipes/kgb_kubeconfig.yaml first-gen basedir=/mnt/data/<group-name>-test

Running the KGB movie recipe
============================

Inside your cult-cargo virtual environment run the following from the resources repository

.. code:: bash

  stimela -C run recipes/luno_movie.yaml recipes/movie_kubeconfig.yaml s3dir=s3://rarg-test-data/group-name-pfb-movie-kgb efsdir=/mnt/data/group-name-test stage=KGB gain-table=/mnt/data/group-name-test/target_init.qc/GKBD-net product=I

Make sure gain-table points to the GKBD-net table that was written when you ran the KGB recipe.


.. _eks: https://aws.amazon.com/eks/
.. _github_join: https://github.com/join
.. _greek_alphabet: https://en.wikipedia.org/wiki/Greek_alphabet
.. _login: https://login.africalim.ratt.center
.. _install_kubectl: https://kubernetes.io/docs/tasks/tools/#kubectl
