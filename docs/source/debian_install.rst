
If working with GPU machines, don't forget to install CUDA and cuDNN before installation.

Install
-------

Currently quick installation is only possible with Ubuntu through the debian packaging system.

.. code:: bash

    # CPU-only package:    
    wget -O poseidon-ubuntu1404_1.0.1_amd64.deb https://github.com/petuum-inc/storage/blob/master/poseidon/deb/ubuntu/cpu/poseidon-ubuntu1404_1.0.1_amd64.deb?raw=true
    sudo dpkg -i poseidon-ubuntu1404_1.0.1_amd64.deb 

    # GPU-enabled package:
    wget -O poseidon-gpu-ubuntu1404_1.0.1_amd64.deb https://github.com/petuum-inc/storage/blob/master/poseidon/deb/ubuntu/gpu/poseidon-gpu-ubuntu1404_1.0.1_amd64.deb?raw=true
    sudo dpkg -i poseidon-gpu-ubuntu1404_1.0.1_amd64.deb

Uninstall
---------

Uninstalling with the debian packaging manager simply means removing the package, called ``poseidon`` or ``poseidon-gpu``:

.. code:: bash

    # CPU-only package:
    sudo apt-get remove poseidon
    
    # GPU-enabled package:
    sudo apt-get remove poseidon-gpu

