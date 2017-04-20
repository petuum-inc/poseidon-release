CUDA Installation
-----------------

TensorFlow uses the Nvidia library CUDA and the machine learning patch cuDNN to run certain computation-heavy operations on GPUs. Thus, in order to launch a Poseidon job you must first install CUDA and cuDNN.

CUDA
^^^^

Refer to `this link <https://developer.nvidia.com/cuda-downloads>`_ to download and install the CUDA toolkit. Download the version 8.0 deb/rpm local package and then follow the installation instructions listed underneath the download link. The default installation will place the toolkit into ``/usr/local/cuda-8.0`` and will create a symbolic folder at ``/usr/local/cuda``.

cuDNN
^^^^^

Download cuDNN v5.1 from `here <https://developer.nvidia.com/cudnn>`_. Choose the appropriate library download for your operating system. The following bash instructions will uncompress and copy the cuDNN files into the toolkit directory. Assuming the toolkit is installed in ``/usr/local/cuda``, run the following commands (edited to reflect the cuDNN version you downloaded):

.. code:: bash

    tar -xvf cudnn-8.0-linux-x64-v5.1-ga.tgz
    sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
    sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
    sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*

Run the following commands to set CUDA environment variables:

.. code:: bash

    export CUDA_HOME="/usr/local/cuda"
    export LD_LIBRARY_PATH="/usr/local/cuda/lib64"

You may also put these commands into ``~/.bashrc`` if you'd like to.


Check NVIDIA Drivers
--------------------

Sometimes GPU device drivers are incorrectly configured. To make sure your GPU drivers are installed properly, run ``nvidia-smi`` on the terminal. A box should show up with the installed GPUs and some stats such as temperature, etc. If this fails, you will need to update to the latest drivers.

If your operating system is CentOS 7.2, you can refer to the `Troubleshooting section <../troubleshoot/#no-nvidia-gpu-device-drivers-found-for-centos-7-2-aws>`_ of the tutorial for advice.

