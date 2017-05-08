For installation, see the `installation instructions <../install/#installation-options>`_.

Quick Tutorial (AWS cluster)
================================

This is a quick tutorial to run a distributed Poseidon task on an AWS cluster. In this tutorial, we will use `CIFAR-10 <http://www.cs.toronto.edu/~kriz/cifar.html>`_, which is a common benchmark in machine learning for image recognition using convolutional neural networks (CNN). More detailed instructions on how to get started are available at: https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/.

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

Setup
^^^^^

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

Execution
^^^^^^^^^

We can now launch Posiedon with the following command. The script, ``cifar10_train.py`` is an example model script included with the Poseidon installation.

.. code:: bash
    
    # The model is in the Poseidon install directory. This line gets the Poseidon home.
    POSEIDON_HOME=`python -c 'import os; import tensorflow; print os.path.dirname(tensorflow.__file__)'`
            
    psd_run -c config.json "python $POSEIDON_HOME/models/image/cifar10/cifar10_train.py --max_steps 1000"

Note that the above script for cifarNet is included in the Poseidon release. If you wish to view the model, it is located in ``$POSEIDON_HOME/models/image/cifar10``.


Poseidon Logs
-------------

After running Poseidon, you can check the execution log ``poseidon_run.log`` in the same path you ran ``psd_run``. There are also output log files for debugging and monitoring purpose created in ``poseidon_log_$TIMESTAMP_SUFFIX`` folder.

Evaluating
----------

Poseidon's evaluating procedure is the same as TensorFlow's. Please follow the tutorial `here <https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/#evaluating_a_model>`_.
