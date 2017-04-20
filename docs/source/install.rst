Installation Instructions
=========================

Supported Operating Systems
---------------------------

* AWS Ubuntu Server 14.04
* AWS Ubuntu Server 16.04
* AWS CentOS 7.x

Pre-Install steps
-----------------

Poseidon runs on GPU clusters. The following steps outline the installation of GPU libraries CUDA and cuDNN:

.. toctree::
   :maxdepth: 2

   cuda_install

Installation Options
--------------------

Poseidon currently supports two ways of installing:

* Debian packaging system (``apt_get``) for clean Ubuntu systems that will not have conflicting dependencies.
* Pip installation: install directly into an existing python2.7 using pip

The debian packaging system is an easier process, and is preferred when installing many machines in a cluster environment. However, if you have a CentOS machine or a customized python setup (such as ``virtualenv``) it would be advisable to use the pip installation method instead.

Ubuntu/Debian Installation
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1

   debian_install

Pip Installation
^^^^^^^^^^^^^^^^
.. toctree::
   :maxdepth: 1

   pip_install

Running on a Cluster
--------------------

Running Poseidon in a cluster environment is simple and is outlined in the next section. Beforehand, install Poseidon using the steps above for each node in your cluster.

