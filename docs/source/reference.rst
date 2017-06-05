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

