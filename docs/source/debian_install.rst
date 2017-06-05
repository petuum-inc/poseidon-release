
Don't forget to install CUDA and cuDNN. Once this is complete, follow these steps.

Install
-------

Currently quick installation is only possible with Ubuntu through the debian packaging system.

.. code:: bash
    
    wget -O poseidon-repo-ubuntu1404_1.0.1_amd64.deb https://github.com/petuum-inc/storage/blob/master/poseidon/deb/ubuntu/poseidon-repo-ubuntu1404_1.0.1_amd64.deb?raw=true
    sudo dpkg -i poseidon-repo-ubuntu1404_1.0.1_amd64.deb

Uninstall
---------

Uninstalling with the debian packaging manager simply means removing the core package, called ``poseidon``:

.. code:: bash
    
    sudo apt-get remove poseidon
    
