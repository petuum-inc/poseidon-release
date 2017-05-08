Reference
=========

Command Line Options for psd_run
--------------------------------

psd_run options "<TensorFlow command>"

Options:

* -c (--cluster-config): path to the json configuration file. More details below.
* -o OUTPUT_FOLDER (--out=OUTPUT_FOLDER): the folder base path where log files will be saved (on each node).

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

Extend TensorFlow Script For Poseidon
-------------------------------------

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

    FLAGS = tf.app.flags.FLAGS
    tf.app.flags.DEFINE_boolean('distributed', False, "Use Poseidon")
    tf.app.flags.DEFINE_string('master_address', "tcp://0.0.0.0:5555", "master address")
    tf.app.flags.DEFINE_integer('client_id', -1, "client id")

When we set config to have ``distributed = True`` this will initiate Poseidon.

Note: ``psd_run`` adds the following flags to the worker when it launches:

* distributed
* master_address
* client_id

If you create a script to launch using ``psd_run``, simply add the FLAGS commands above and create a ``ConfigProto`` as above and the runner will automatically launch using your specifications. The above defaults are useful because when you launch without specifying ``distributed=True``, it will by default run like a native 1 node TensorFlow application.

This table demonstrates the Poseidon settings, and ``psd_run`` defaults.

.. list-table::
   :widths: auto
   :align: center
   :header-rows: 1

   * - Arg Name
     - Set by psd_run
     - Default
   * - distributed
     - True
     - True 
   * - master_address
     - True
     - master_address in json or localhost
   * - client_id
     - True
     - 0 -> N-1 (N is number of workers)
   * - num_push_threads
     - False
     - 1
   * - num_pull_threads
     - False
     - 1
   * - block_size
     - False
     - 4 MB
   * - use_sfb
     - False
     - False
