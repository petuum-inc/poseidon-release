# Building Poseidon Wheels

## Supported Operating Systems
- AWS Ubuntu Server 14.04
- AWS Ubuntu Server 16.04
- AWS CentOS 6.x(TODO)
- AWS CentOS 7.x
- AWS OSX EI Caption(TODO)
- AWS OSX Sierra

## Dependencies
### Install CUDA and cuDNN
Refer to this [link](https://developer.nvidia.com/cuda-downloads) to download and install CUDA. Please install version 8.0. For Ubuntu, please directly download the `.deb` file and do installation. For CentOS and redHat, please directly download the `.rpm` file and do installation. For Macintosh, please directly download the `.dmg` file and do installation. The default installation will put the toolkit into `/usr/local/cuda-8.0` and will create a symbolic folder at `/usr/local/cuda`.

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

### Install Bazel
For CentOS or Redhat, TODO

Follow instructions [here](http://bazel.build/docs/install.html) to **install Bazel with Installer**.

Please make sure install Bazel-0.3.1 **(Other versions do NOT work)** by first installing Bazel dependencies, and then downloading [Bazel-0.3.1 installer for your system](https://github.com/bazelbuild/bazel/releases), and finally running the installer as mentioned there:
```
$ chmod +x PATH_TO_INSTALL.SH
$ ./PATH_TO_INSTALL.SH --user
```
Remember to replace **PATH_TO_INSTALL.SH** with the location where you downloaded the installer.

As we ran the Bazel installer with the --user flag as above, the Bazel executable is installed in the $HOME/bin directory. It's a good idea to add this directory to the default paths, as follows:
```
$ export PATH="$PATH:$HOME/bin"
```

### Install other dependencies
```
# Redhat/CentOS 7.x 64-bit
sudo yum install wget
sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -ivh epel-release-latest-7.noarch.rpm
sudo yum makecache
sudo yum install swig python-pip python-devel.x86_64 numpy.x86_64 python-wheel
sudo yum install git
sudo pip install -U numpy

# Ubuntu/Linux 64-bit
sudo apt-get install swig python-pip python-numpy python-dev python-wheel
sudo apt-get -y install git

# OSX
brew install swig python homebrew/python/numpy
sudo pip install wheel
brew install git
```

### Install Protobuf
```
sudo pip install setuptools==30.1.0 protobuf
```

## Compiling Poseidon Wheels
### Download Source Code
```
git clone -b d0.10-aws https://petuum.githost.io/internal/poseidon.git
```

Run the **configure** script at the root of the tree. The configure script asks you for the path to your python interpreter and allows (optional) configuration of the CUDA libraries.

This step is used to locate the python and numpy header files as well as enabling GPU support if you have a CUDA enabled GPU and Toolkit installed. Select the option Y when asked to build TensorFlow with GPU support.

If you have several versions of Cuda or cuDNN installed, you should definitely select one explicitly instead of relying on the system default.

For example:
```
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] N
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with GPU support? [y/N] y
GPU support will be enabled for TensorFlow
Please specify which gcc nvcc should use as the host compiler. [Default is /usr/bin/gcc]:
Please specify the Cuda SDK version you want to use, e.g. 7.0. [Leave empty to use system default]: 8.0
Please specify the location where CUDA 8.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify the cuDNN version you want to use. [Leave empty to use system default]: 5
Please specify the location where cuDNN 5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size.

Setting up Cuda include
Setting up Cuda lib
Setting up Cuda bin
Setting up Cuda nvvm
Setting up CUPTI include
Setting up CUPTI lib64
Configuration finished
```

This creates a canonical set of symbolic links to the Cuda libraries on your system. Every time you change the Cuda library paths you need to run this step again before you invoke the bazel build command. For the cuDNN libraries, use '7.0' for R3, and '4.0.7' for R4.

When building from source, you will still build a pip package and install that.

Please note that building from sources takes a lot of memory resources by default and if you want to limit RAM usage you can add **--local_resources 2048,.5,1.0** while invoking bazel.

```
$ bazel build -c opt //tensorflow/tools/pip_package:build_pip_package

# To build with GPU support:
$ bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package

$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/poseidon_pkg

# publish: TODO
$
```
