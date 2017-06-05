Poseidon on AWS Cluster
=======================

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

Note: replace ``cluster-key.pem`` with a path to your AWS pem file.

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

Note: if running on multiple AWS nodes, add each node's private IP in the worker_nodes and server_nodes lists within config.json.

Execution
^^^^^^^^^

We can now launch Posiedon with the following command.

.. code:: bash
    
    psd_run -c config.json "python $TF_MODEL_HOME/tutorial/image/cifar10/cifar10_train.py --max_steps 1000"

Poseidon Logs
-------------

After running Poseidon, you can check the execution log ``poseidon_run.log`` in the same path you ran ``psd_run``. There are also output log files for debugging and monitoring purpose created in ``poseidon_log_$TIMESTAMP_SUFFIX`` folder.



