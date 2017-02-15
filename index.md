# Welcome to Poseidon

Poseidon is a highly scalable and efficient system architecture for large-scale deep learning on GPU clusters. This is the document website for Poseidon release.

## Quick Start:

[Install Poseidon](http://poseidon-release.readthedocs.io/en/latest/User_Installation/)

[Start your first distributed training instance using Poseidon](http://poseidon-release.readthedocs.io/en/latest/Quick_Tutorial/)

# Introduction

Poseidon is an efficient communication interface for data-parallel DL on distributed GPUs. Poseidon exploits the sequential layer-by-layer structure in DL programs, finding independent GPU computation operations and network communication operations in the training algorithm, so that they can be scheduled together to reduce bursty network communication. Moreover, Poseidon implements a hybrid communication scheme that accounts for each DL program layer's mathematical properties as well as the cluster configuration, in order to compute the network cost of different communication methods, and select the cheapest one -- currently, Poseidon implements and supports a [parameter server scheme] (http://www.pdl.cmu.edu/PDL-FTP/BigLearning/CMU-PDL-15-105.pdf) that is well-suited to small matrices, and a [sufficient factor broadcasting scheme] (http://www.cs.cmu.edu/~pengtaox/papers/uai16_sfb.pdf) that performs well on large matrices.

Poseidon is orthogonal to TensorFlow, Caffe -- the techniques present in Poseidon could be used to produce a better distributed version of any existing DL frameworks. This release includes Poseidon-enabled Tensorflow, a better, more efficient and more scalable distributed deep learning system that has TensorFlow as the computing engine.

# Performance at a Glance

## Throughput Scalability

Poseidon-enabled TensorFlow scale almost-linearly in algorithm throughput with additional machines, while incurring little additional overhead even in the single machine setting. 

The following figure shows Poseidon's performance on scaling up four widely adopted neural networks (see the table for their configurations) using distributed GPU clusters. 

<img src="https://c1.staticflickr.com/3/2632/32079844734_79b632baa7_n.jpg" height="300"> 
<img src="https://c1.staticflickr.com/3/2344/32079474554_9f2fd0ff3b_n.jpg" height="300"> 

<img src="https://c1.staticflickr.com/3/2106/32079474524_2f9df5b1a9_n.jpg" height="300">
<img src="https://c1.staticflickr.com/4/3900/32769057602_dcc944d4a5_n.jpg" height="300">

| Name| # parameters| Dataset | Batchsize (single node) |
| :---:|:---:|:---:|:---:|
| _Inception-V3_  | 27M | ILSVRC12  | 32 |
| _VGG19_ | 143M | ILSVRC12 | 32 |
| _VGG19-22K_ | 229M | ILSVRC12  | 32 | 
| _ResNet-152_ | 60.2M | ILSVRC12 | 32 |


For distributed execution, Poseidon consistently delivers near-linear increases in throughput across various models and engines: e.g. 31.5x speedup on training the Inception-V3 network using TensorFlow engine on 32 nodes, which improves 50% upon the original TensorFlow (20x); When training a 229M parameter network (VGG19-22K), Poseidon still achieves near-linear speedup (30x on 32 nodes) using TensorFlow engine, while distributed TensorFlow sometimes experiences negative scaling with additional machines. 

# Older version of Poseidon

You can find the Poseidon-enabled Caffe in [this repository](https://github.com/sailing-pmls/poseidon/wiki).

# Reference
      
[Poseidon: A System Architecture for Efficient GPU-based Deep Learning on Multiple Machines](https://arxiv.org/pdf/1512.06216v1.pdf)

# Contact
[hao@cs.cmu.edu](mailto:hao@cs.cmu.edu), [xunzhangthu@gmail.com](mailto:xunzhangthu@gmail.com)
