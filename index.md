## Welcome to Poseidon

Poseidon is a highly scalable and efficient system architecture for large-scale deep learning on GPU clusters. This is the document website for Poseidon release.

### Quick Start:

[Install Poseidon]
(http://poseidon-release.readthedocs.io/en/latest/User_Installation/)

[Start your first distributed training instance]
(http://poseidon-release.readthedocs.io/en/latest/Quick_Tutorial/)

## Introduction
Poseidon is an efficient communication interface for data-parallel DL on distributed GPUs. Poseidon exploits the sequential layer-by-layer structure in DL programs, finding independent GPU computation operations and network communication operations in the training algorithm, so that they can be scheduled together to reduce bursty network communication. Moreover, Poseidon implements a hybrid communication scheme that accounts for each DL program layer's mathematical properties as well as the cluster configuration, in order to compute the network cost of different communication methods, and select the cheapest one -- currently, Poseidon implements and supports a [parameter server scheme] (http://www.pdl.cmu.edu/PDL-FTP/BigLearning/CMU-PDL-15-105.pdf) that is well-suited to small matrices, and a [sufficient factor broadcasting scheme] (http://www.cs.cmu.edu/~pengtaox/papers/uai16_sfb.pdf) that performs well on large matrices.

 Poseidon is orthogonal to TensorFlow, Caffe -- the techniques present in Poseidon could be used to produce a better distributed version of any existing DL frameworks. This release includes Poseidon-enabled Tensorflow, a better, more efficient and more scalable distributed deep learning system that has TensorFlow as its computing engine.

## Performance at a Glance



## Older version of Poseidon

You can find the Poseidon-enabled Caffe in [this repository] (https://github.com/sailing-pmls/poseidon/wiki)

## Reference
      
[Poseidon: A System Architecture for Efficient GPU-based Deep Learning on Multiple Machines](https://arxiv.org/pdf/1512.06216v1.pdf)

## Contact
[hao@cs.cmu.edu](mailto:hao@cs.cmu.edu), [xunzhangthu@gmail.com](mailto:xunzhangthu@gmail.com)
