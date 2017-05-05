For installation, see the `installation instructions <../install/#installation-options>`_.

Quick Tutorial (private cluster)
================================

This is a quick tutorial to run a distributed Poseidon task on a private cluster. In this tutorial, we will use `CIFAR-10 <http://www.cs.toronto.edu/~kriz/cifar.html>`_, which is a common benchmark in machine learning for image recognition using convolutional neural networks (CNN). More detailed instructions on how to get started are available at: https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/.

Data
----

The dataset will download automatically when you run the training code with the default options. You can also put your dataset in a distributed file system.

Training
--------

The executable for running Poseidon tasks is ``psd_run``. Running ``psd_run -h`` should print this:

.. code::

    usage: psd_run [-h] [-c,--cluster_config CLUSTER_CONFIG]
                   [-o,--out OUTPUT_FOLDER]
                   cmd

    psd_run runs Poseidon for distributed machine learning on a GPU cluster. The following are its command line arguments.

    positional arguments:
      cmd

    optional arguments:
      -h, --help            show this help message and exit
      -c,--cluster_config CLUSTER_CONFIG
                            configuration file for cluster environment: specify workers in each machine. See example: 
                            {
                              "virtualenv": "/home/ubuntu/sandbox",
                              "username": "ubuntu",
                              "pem_file": "/path/to/pem-file.pem",
                              "master_node": "localhost",
                              "worker_nodes": [
                                "192.168.1.11",
                                "192.168.1.15"
                              ],
                              "server_nodes": [
                                "192.168.1.70",
                                "192.168.1.80"
                              ]
                            }
      -o,--out OUTPUT_FOLDER
                            output log folder

Say we wish to run Poseidon on two nodes, IP1 and IP2. We must create a ``config.json`` to specify our configurations. The runner ``psd_run`` uses ssh to communicate with the cluster, so certain options must be added, such as username.

.. code:: json

    {
      "username": "<cluster username>",
      "worker_nodes": [
        "<IP1>",
        "<IP2>"
      ],
      "server_nodes": [
        "<IP1>",
        "<IP2>"
      ]
    }

For security reasons, the script does not allow passwords in ssh. Therefore, no-password ssh must be enabled for the username. On <IP1> (our master node), generate a key-pair. Then copy it onto <IP2>:

.. code:: bash

    ssh-keygen
    # Use defaults (press ENTER three times)
    
    ssh-copy-id -i ~/.ssh/id_rsa.pub <cluster username>@<IP2>
    # Enter password

    # Test, ssh should not prompt for a password
    ssh <cluster username>@<IP2>
    exit

We can now test Posiedon with the following command. The script, ``cifar10_train.py`` is an example model script included with the Poseidon installation.

.. code:: bash
    
    POSEIDON_HOME=`python -c 'import os; import tensorflow; print os.path.dirname(tensorflow.__file__)'`
    psd_run -c config.json "python $POSEIDON_HOME/models/image/cifar10/cifar10_train.py --max_steps 1000"


Poseidon Logs
-------------

After running Poseidon, you can check the execution log ``poseidon_run.log`` in the same path you ran ``psd_run``. There are also output log files for debugging and monitoring purpose created in ``poseidon_log_$TIMESTAMP_SUFFIX`` folder.

Evaluating
----------

Poseidon's evaluating procedure is the same as TensorFlow's. Please follow the tutorial `here <https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/#evaluating_a_model>`_.
