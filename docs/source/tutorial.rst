For installation, see the `installation instructions <../install/#installation-options>`_.

Quick Tutorial
==============

This is a quick tutorial to run a distributed Poseidon task. In this tutorial, we will use `CIFAR-10 <http://www.cs.toronto.edu/~kriz/cifar.html>`_, which is a common benchmark in machine learning for image recognition using convolutional neural networks (CNN). More detailed instructions on how to get started are available at: https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/.

Data
----

The dataset will download automatically when you run the training code with the default options. You can also put your dataset in a distributed file system.

Training
--------

The executable for running Poseidon tasks is ``psd_run``. Running ``psd_run -h`` should print this:

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


We must create a ``config.json`` to specify our cluster configurations. If running a single node on Ubuntu within an AWS instance (with no virtualenv), the configurations are very simple:

.. code:: json

    {
      "pem_file": "cluster-key.pem",
      "worker_nodes": [
        "127.0.0.1"
      ],
      "server_nodes": [
        "127.0.0.1"
      ]
    }

Note: replace ``cluster-key.pem`` with a path to your AWS pem file.

Download the Poseidon tutorial script into the same directory as ``config.json``:

https://raw.githubusercontent.com/petuum-inc/poseidon-release/v0.10/models/cifar10/cifar10_train.py

Note: this is almost the same script with the native tensorflow program. The only difference is that we add following few lines of code which will pass the arguments to Poseidon backend.

.. code:: python
  
  tf.app.flags.DEFINE_boolean('distributed', False, "Mode.")
  tf.app.flags.DEFINE_string('master_address', "tcp://0.0.0.0:5555", "master address")
  # in train() function
  if FLAGS.distributed:
    config.distributed = FLAGS.distributed
    config.master_address = FLAGS.master_address
    config.client_id = FLAGS.client_id

We can now test Posiedon on a single node with the following command:

.. code:: bash

    psd_run -c config.json "python cifar10_train.py --max_steps 100"


Poseidon Logs
-------------

After running Poseidon, you can check the execution log ``poseidon_run.log`` in the same path you ran ``psd_run``. There are also output log files for debugging and monitoring purpose created in ``poseidon_log_$TIMESTAMP_SUFFIX`` folder.

Evaluating
----------

Poseidon's evaluating procedure is the same as TensorFlow's. Please follow the tutorial `here <https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/#evaluating_a_model>`_.
