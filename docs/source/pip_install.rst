
Don't forget to install CUDA and cuDNN, more details can be found `here <../cuda_install/>`_. Once this is complete, follow these steps.

Setup
-----

Installation using pip will work on many Unix machines. We support Ubuntu and CentOS (7.x).

First, install ``pip`` as well as some OS specific core dependencies:

.. code:: bash
    
    # Redhat/CentOS 7.x 64-bit
    sudo yum install wget
    sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    sudo rpm -ivh epel-release-latest-7.noarch.rpm
    sudo yum makecache
    sudo yum install python-pip libffi-devel python-devel openssl-devel
    
    # Ubuntu 14.04 and 16.04 64-bit
    sudo apt-get update
    sudo apt-get install python-pip libssl-dev python-dev libffi-dev



Virtualenv (Optional)
---------------------

Use virtualenv if you wish to install poseidon and tensorflow to a clean python environment.

Note: if you already have tensorflow installed on the machine, this is highly recommended.

The following will install virtualenv and set up a virtual python environment under ``~/sandbox``:

.. code:: bash

    sudo pip install virtualenv
    mkdir ~/sandbox
    cd ~/sandbox
    virtualenv .
    . bin/activate

Once ``activate`` has been run, virtualenv sets ``~/sandbox/bin`` as the first path for bash. Now ``which python`` and ``which pip`` should point to this directory, and anything installed using ``pip`` will go here.

Note: to uninstall virtualenv, all you have to do is remove the sandbox.

.. code:: bash

    deactivate # If you are using a bash that has been 'activate'd
    rm -r ~/sandbox


Install
-------

Note: if you are not using virtualenv, you may need to ``sudo`` the following instructions.

.. code:: bash

    pip install --upgrade setuptools==30.1.0 protobuf==3.1.0 numpy paramiko

    pip install https://github.com/petuum-inc/storage/blob/master/poseidon/wheel/linux/gpu/poseidon-0.10.0-cp27-none-linux_x86_64.whl?raw=true

Uninstall
---------

To uninstall, run the following command:

.. code:: bash

    pip uninstall poseidon

