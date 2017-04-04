## Supported Operating Systems
  - AWS Ubuntu Server 14.04
  - AWS Ubuntu Server 16.04
  - AWS CentOS 6.x(TODO)
  - AWS CentOS 7.x(not up-to-date)
  - AWS OSX EI Caption(not up-to-date)
  - AWS OSX Sierra(not up-to-date)


## CUDA and CUDNN Requirements

Currently, Poseidon only supports GPU environments so you need to install cuda and cudnn library. 

Determine if your GPU driver is correctly configured (for Nvidia GPU's) with `nvidia-smi` command. A box should show up with the installed GPUs and some stats such as temperature, etc. If this fails refer to the GPU driver instructions in the Known Issues page at the bottom of this tutorial.

Refer to this [link](https://developer.nvidia.com/cuda-downloads) to download and install CUDA. Please install version 8.0. For Ubuntu 14.04, please directly download the `.deb` file and do installation. For CentOS and redHat, please directly download the  .rpm file and do installation. For Macintosh, please directly download the .dmg file and do installation. The default installation will put the toolkit into `/usr/local/cuda-8.0` and will create a symbolic folder at `/usr/local/cuda`.

Download cuDNN v5.1 from [here](https://developer.nvidia.com/cudnn). Uncompress and copy the cuDNN files into the toolkit directory. Assuming the toolkit is installed in /usr/local/cuda, run the following commands (edited to reflect the cuDNN version you downloaded):
```
tar -xvf cudnn-8.0-linux-x64-v5.1-ga.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

Then run the following commands to set the environment variables:
```
export CUDA_HOME="/usr/local/cuda"
export LD_LIBRARY_PATH="/usr/local/cuda/lib64"
```
You may also put these commands into `~/.bashrc` if you'd like to.

## Install using pip

- Install python-pip (if you haven't already done so)

```
# Redhat/CentOS 7.x 64-bit
sudo yum install wget
sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -ivh epel-release-latest-7.noarch.rpm
sudo yum makecache
sudo yum install python-pip libffi-devel python-devel openssl-devel

# Ubuntu 14.04 and 16.04 64-bit
sudo apt-get update
sudo apt-get install python-pip

# OSX
brew install swig python homebrew/python/numpy
sudo pip install wheel paramiko
brew install git
```

#### OPTIONAL: Set up virtualenv

Use virtualenv if you wish to install poseidon and tensorflow to a clean python environment. 

Note: if you already have tensorflow installed on the machine, this is highly recommended.

- Install virtualenv to new directory (for instance, ~/poseidon)
```
sudo pip install virtualenv
mkdir ~/poseidon
cd ~/poseidon
virtualenv .
. bin/activate

# Uninstall
deactivate
sudo rm -r ~/poseidon
```

At this point, your terminal is set up to install into the new location.

#### Install dependencies and poseidon
Note: if you are not using virtualenv, you may need to `sudo` the following operations.

- Install setuptools, protobuf, numpy
```
sudo apt-get install libssl-dev
pip install --upgrade setuptools==30.1.0 protobuf==3.1.0 numpy paramiko
```

- Install Poseidon
```
# Linux/gpu
export PSD_BINARY_URL=https://github.com/sailing-pmls/storage/blob/master/poseidon/wheel/linux/gpu/poseidon-0.10.0-cp27-none-linux_x86_64.whl?raw=true

# OSX/cpu
export PSD_BINARY_URL=https://github.com/petuum/storage/blob/master/poseidon/wheel/mac/cpu/poseidon-0.10.0-py2-none-any.whl?raw=true

# Install
pip install $PSD_BINARY_URL
```

Uninstall Posieon:
```
pip uninstall poseidon
```

## Install using apt-get under ubuntu/debian
First, you need to install CUDA and CUDNN, please check out the install guide above.

```
wget -O poseidon-repo-ubuntu_0.10_amd64.deb https://github.com/sailing-pmls/storage/blob/master/poseidon/deb/ubuntu/poseidon-repo-0.10_amd64.deb?raw=true
sudo dpkg -i poseidon-repo-ubuntu_0.10_amd64.deb
sudo apt-get update
sudo apt-get install poseidon
# uninstall
sudo apt-get remove poseidon
```

## Cluster Installation
To deploy Poseidon in a cluster environment, please follow the steps above for each cluster node. You can also use package tools under your system to do cluster deployment such as portage(Gentoo), deb(Ubuntu, Debian), rpm(CentOS, Redhat). We will support these kinds of package tools soon.

Note: If you installed poseidon using virtualenv, make sure to specify which python should run the scripts by launching python with the absolute path. For instance, if your username is ubuntu and you placed virtualenv in ~/poseidon:

```
/home/ubuntu/poseidon/bin/python script.py --args
```

## Tiny Test
You can now test your installation:
```
$ python
...
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, Poseidon!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
Hello, Poseidon!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print(sess.run(a + b))
42
>>>
```

## Known Issues

- Pip no connection

Please try to add a proxy with the pip command:
```
sudo pip --proxy http://web-proxy.mydomain.com install somepackage
```

- No installed Nvidia GPU driver
Device driver installation instructions for CentOS 7

```
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
sudo yum install kmod-nvidia
```

Finally, reboot.

Try nvidia-smi again to verify the installation was successful.

