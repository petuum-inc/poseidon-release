## Supported Operating Systems
  - AWS Ubuntu Server 14.04
  - AWS Ubuntu Server 16.04
  - AWS CentOS 6.x(TODO)
  - AWS CentOS 7.x
  - AWS OSX EI Caption
  - AWS OSX Sierra


## CUDA and CUDNN Requirements

Currently, Poseidon only supports GPU environment so you need to install cuda and cudnn library. Refer to this [link](https://developer.nvidia.com/cuda-downloads) to download and install CUDA. Please install version 8.0. For Ubuntu 14.04, please directly download the `.deb` file and do installation. For CentOS and redHat, please directly download the  .rpm file and do installation. For Macintosh, please directly download the .dmg file and do installation. The default installation will put the toolkit into `/usr/local/cuda-8.0` and will create a symbolic folder at `/usr/local/cuda`.

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
- Install python-pip, python-numpy
```

# Redhat/CentOS 7.x 64-bit
sudo yum install wget
sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -ivh epel-release-latest-7.noarch.rpm
sudo yum makecache
sudo yum install swig python-pip numpy.x86_64 python-wheel
sudo yum install git

# Ubuntu 14.04 64-bit
sudo apt-get update
sudo apt-get install python-pip python-numpy python-paramiko

# Ubuntu 16.04 64-bit
sudo apt-get update
sudo apt install python-pip python-numpy python-paramiko

# OSX
brew install swig python homebrew/python/numpy
sudo pip install wheel paramiko
brew install git
```

- Install protobuf
```
sudo pip install --upgrade protobuf==3.1.0 setuptools==30.1.0
```

- Install Poseidon
```
# Linux/gpu
export PSD_BINARY_URL=https://github.com/sailing-pmls/storage/blob/master/poseidon/wheel/linux/gpu/poseidon-0.10.0-cp27-none-linux_x86_64.whl?raw=true

# OSX/cpu
export PSD_BINARY_URL=https://github.com/petuum/storage/blob/master/poseidon/wheel/mac/cpu/poseidon-0.10.0-py2-none-any.whl?raw=true
```

Install Poseidon:
```
sudo pip install $PSD_BINARY_URL
```

## Install using apt-get under ubuntu/debian
```
wget https://github.com/sailing-pmls/storage/raw/master/poseidon/deb/ubuntu14/poseidon-repo-ubuntu0.10_amd64.deb
sudo dpkg -i poseidon-repo-ubuntu0.10_amd64.deb
sudo apt-get update
sudo apt-get install poseidon
```

## Cluster Installation
To deploy Poseidon in a cluster environment, please follow the steps above for each cluster node. You can also use package tools under your system to do cluster deployment such as portage(Gentoo), deb(Ubuntu, Debian), rpm(CentOS, Redhat). We will support these kinds of package tools soon.

For AWS users, we provide the AMIs for Ubuntu14.04(search for `poseidon-v0.10-aws-ubuntu14.04LTS` under `Community AMIs`) and Ubuntu16.04(search for `poseidon-v0.10-aws-ubuntu16.04LTS` under `Community AMIs`), you can just lauch your instances directly using these AMIs.

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

Please try to add a prxoy with the pip command:
```
sudo pip --proxy http://web-proxy.mydomain.com install somepackage
```
