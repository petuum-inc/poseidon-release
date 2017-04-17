Installation Instructions
=========================

Supported Operating Systems
---------------------------

* AWS Ubuntu Server 14.04
* AWS Ubuntu Server 16.04
* AWS CentOS 7.x

Pre-Install steps
-----------------

Poseidon currently only runs on GPU clusters. CUDA and cuDNN packages are required before installation.

.. toctree::
   :maxdepth: 2

   cuda_install

Installation Options
--------------------

Poseidon currently supports two ways of installing:

* Debian packaging system (``apt_get``) for clean Ubuntu systems that will not have conflicting dependencies.
* Pip installation: install directly into an existing python2.7 using pip

The debian packaging system is an easier process, and is preferred when installing many machines in a cluster environment. However, if you have a CentOS machine or a complicated python setup (such as ``virtualenv``) it would be advisable to use the pip installation method instead.

Ubuntu/Debian Installation
^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1

   debian_install

Pip Installation
^^^^^^^^^^^^^^^^
.. toctree::
   :maxdepth: 1

   pip_install

Cluster Installation
--------------------

To deploy Poseidon in a cluster environment, please follow the steps above for each cluster node.

Note: If you installed poseidon using virtualenv, make sure to specify which python should run the scripts by launching python with the absolute path. For instance, if your username is ubuntu and you placed virtualenv in ``~/sandbox``:

.. code:: bash
    /home/ubuntu/sandbox/bin/python script.py --args
