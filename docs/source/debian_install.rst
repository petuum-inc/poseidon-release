
Don't forget to install CUDA and cuDNN. Once this is complete, follow these steps...

Install
-------

Currently quick installation is only possible with Ubuntu through the debian packaging system.

This creates an ``apt-get`` repository on your system with a few dependencies in it as local ``deb`` files. The final step is to install the core Poseidon project, and it will automatically install all the dependencies.

.. code:: bash
    
    wget -O poseidon-repo-ubuntu_0.10_amd64.deb https://github.com/sailing-pmls/storage/blob/master/poseidon/deb/ubuntu/poseidon-repo-0.10_amd64.deb?raw=true
    sudo dpkg -i poseidon-repo-ubuntu_0.10_amd64.deb
    sudo apt-get update
    sudo apt-get install poseidon

Uninstall
---------

Uninstalling with the debian packaging manager simply means removing the core package, called ``poseidon``:

.. code:: bash
    
    sudo apt-get remove poseidon

