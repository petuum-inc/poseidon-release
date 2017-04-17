For installation, see the `installation instructions <../install/#installation-options>`_.

Quick Tutorial
==============

This is a quick tutorial to run a distributed Poseidon task. In this tutorial, we will use `CIFAR-10 <http://www.cs.toronto.edu/~kriz/cifar.html>`_, which is a common benchmark in machine learning for image recognition using convolutional neural networks (CNN). More detailed instructions on how to get started are available at: https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/.

Data
----

The dataset will be downloaded automatically when you run the training code. In other words, the dataset is shared in the server across network. You can also put your dataset in a distributed file system or in the same system path.

Training
--------

Now, suppose you have Poseidon installed at ``/usr/local/lib/python2.7/dist-packages/tensorflow``, we will call it ``$POSEIDON_HOME`` below. Note that the namespace is 'tensorflow', to minimize the modification effort if porting to Posiedon from native TensorFlow. The main entry to run Poseidon tasks is ``psd_run``.

.. code::

    Usage: 
      psd_run options args

    Example:
      psd_run -c(--cluster_config) -o(--out) ~/logs config.json "python conv.py --batch 10"

    Options:
      -h, --help       show this help message and exit
      -c CLUSTER_CONFIG, --cluster_config=CLUSTER_CONFIG
                       configuration file for cluster environment: specify
                       workers in each machine.
                       See example:  {
                       "pem_file": "cluster-key.pem",
                       "username": "ubuntu",
                       "worker_nodes": [
                           "192.168.1.11",
                           "192.168.1.12",
                           "192.168.1.15"],
                       "server_nodes": [
                           "192.168.1.70",
                           "192.168.1.80",
                           "192.168.1.90"]
                       }
      -o OUTPUT_FOLDER, --out=OUTPUT_FOLDER
                       output log folder


Poseidon Logs
-------------

Evaluating
----------


