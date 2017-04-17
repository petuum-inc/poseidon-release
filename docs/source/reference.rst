Reference for psd_run
=====================

Poseidon has three different processes that can be run from any machine using the configuration json file.

* master - the ``ps_master`` executable stores all nodes in a table and gives nodes information upon request
* parameter server - the ``ps_server`` executable runs a parameter server that can coordinate with workers. Its main purpose is to store a key-pair table of gradients and wait for synchronization
* workers - these are launched in the same way as a native TensorFlow script would be. The only difference is that the TensorFlow session object must be initialized with some Poseidon-specific configurations. The workers maintain their own separate computation graphs and begin execution. When they run into gradient tensors, they synchronize with the ps_servers.

The runner script ``psd_run`` launches all the processes. By default, it starts the ``ps_master`` on the same node (localhost), and starts all of the ip's configured in the configuration json via ``ssh``. Each process (on each node) saves logs in the same path location. The master redirects to ``master.log``, the servers redirect to ``server<k>.log`` where k is an integer starting at 0, and the workers redirect to ``worker<k>.log``. The workers will store the same output as a native TensorFlow execution would.

Since the workers are simply native TensorFlow applications with a modified session initialization, refer to the `TensorFlow api_docs <https://www.tensorflow.org/versions/r0.10/api_docs/python/>`_ for reference.

Command Line Options
--------------------

psd_run options "<TensorFlow command>"

Options:

* -c (--cluster-config): path to the json configuration file. More details below.
* -o OUTPUT_FOLDER (--out=OUTPUT_FOLDER): the folder base path where log files will be saved.

JSON Cluster Config
-------------------

Within the cluster configuration json there are two required fields and several optional ones.

Required:

* worker_nodes - required list of IP addresses to run the tensorflow script files.
* server_nodes - required list of IP addresses for parameter servers.
Note: The number of worker and server nodes currently must be equal.

Optional:

* master_node - IP of the master, by default this is set to be local machine
* pem_file - If using aws, use this to point to the pem file required for ssh auth
* virtualenv - If using python within a virtualenv, point to the virtualenv root directory (equivalent to  echo $VIRTUAL_ENV).
* username - Set global username for all processes/communication. The username should be set for ssh no-password authentication on all machines. The default is ubuntu.

Poseidon session initialization args
------------------------------------

Posiedon worker scripts should be almost identical to native TensorFlow, but with an extended session initialization object.

In native TensorFlow, you initialize a session like this:

.. code:: python

    import tensorflow as tf
    sess = tf.session(config = None)
    ...

Poseidon extends the optional config object with several parameters like this:

.. code:: python
    
    config = tf.ConfigProto(log_device_placement = FLAGS.log_device_placement)
    config.master_address = FLAGS.master_address
    config.client_id = FLAGS.client_id
    config.num_push_threads = 1
    config.num_pull_threads = 1
    config.distributed = FLAGS.distributed
    sess = tf.Session(config = config)
    ...

``FLAGS`` here comes from ``tf.app.flags``. The three FLAGS parameters above could be configured like this:

.. code:: python

    tf.app.flags.DEFINE_boolean('distributed', False, "Use Poseidon")
    tf.app.flags.DEFINE_string('master_address', "tcp://0.0.0.0:5555", "master address")
    tf.app.flags.DEFINE_integer('client_id', -1, "client id")

When we set config to have ``distributed = True`` this will initiate Poseidon.

Note: ``psd_run`` adds the following flags to the worker when it launches:

* distributed
* master_address
* client_id

If you create a script to launch using ``psd_run``, simply add the FLAGS commands above and create a ``ConfigProto`` as above and the runner will automatically launch using your specifications. The above defaults are useful because when you launch without specifying ``distributed=True``, it will by default run as a native TensorFlow application.

This table demonstrates the Poseidon settings, and ``psd_run`` defaults.

.. list-table::
   :widths: 10 10 20 40
   :align: center
   :header-rows: 1

   * - Arg Name
     - Set by psd_run
     - Default
     - Explanation
   * - distributed
     - True
     - True 
     - Run using Poseidon
   * - master_address
     - True
     - master_address or localhost
     - The connection string to ps_master node
   * - client_id
     - True
     - 0 -> N-1 (N is number of workers)
     - The id of the particular worker
   * - num_push_threads
     - False
     - 4
     - scale factor: workers to servers
   * - num_pull_threads
     - False
     - 8
     - scale factor: servers to workers
   * - block_size
     - False
     - 4 MB
     - tensors will be split over this size
   * - use_sfb
     - False
     - False
     - Use Sufficient Factor Broadcasting hybrid protocol

