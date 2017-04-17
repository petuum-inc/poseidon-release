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

Posiedon worker scripts should be almost identical to native TensorFlow execution scripts, but with an extended session initialization object.
