Troubleshooting
===============

No Nvidia GPU device drivers found for CentOS 7.2 (AWS)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can check if your drivers are installed correctly by calling ``nvidia-smi`` via the terminal. If the drivers are installed, this should result with a box showing GPU temperature, processes, etc. If it returns in error, then you will need to install (or update) your Nvidia drivers.

Try the below command. Advisable only on fresh instance, it updates the kernel to a newer version that is supported under Nvidia device drivers.

.. code:: bash
    
    sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
    sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
    sudo yum install kmod-nvidia

Finally, reboot.

Try ``nvidia-smi`` to verify the installation was successful.
