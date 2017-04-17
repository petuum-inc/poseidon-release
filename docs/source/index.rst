
Welcome to Poseidon
===================

Poseidon is a highly scalable and efficient system architecture for large-scale deep learning on GPU clusters. This is the document website for Poseidon-Tensorflow Release 0.10.

The Poseidon engine is a modification of TensorFlow that keeps the `Tensorflow client API <https://www.tensorflow.org/versions/r0.10/>`_ intact.

Introduction
------------

Poseidon is a communication interface built into TensorFlow. Its purpose is to allow deep learning applications written and tested on single GPU nodes to easily scale onto a cluster environment. Poseidon exploits the sequential layer-by-layer structure common in deep learning architectures. It separates GPU computation and network communication operations in the training algorithm into a pipeline architecture to reduce network bottlenecks. Currently, Poseidon implements and supports a `parameter server scheme <http://www.pdl.cmu.edu/PDL-FTP/BigLearning/CMU-PDL-15-105.pdf>`_ that is well-suited to small matrices, and a `sufficient factor broadcasting scheme <http://www.cs.cmu.edu/~pengtaox/papers/uai16_sfb.pdf>`_ that performs well on large matrices. It can calculate which of these schemes will be the most efficient for each layer by taking into account each layer's size and the number of nodes in the cluster.

The techniques present in Poseidon could be used to produce a better distributed version for any existing deep learning framework. This release includes Poseidon-enabled TensorFlow 0.10, a more efficient, scalable distributed deep learning system than native TensorFlow, but having TensorFlow as the computing engine.

Contents
--------
.. toctree::
   :maxdepth: 4

   install
   tutorial
   reference
   known_issues

Performance at a Glance
-----------------------

Poseidon-enabled TensorFlow can scale almost linearly in total throughput with additional machines while simultaneously incurring little additional overhead.

The following figures show Poseidon's performance on four widely adopted neural networks (see the table for their configurations) using distributed GPU clusters.

.. image:: https://c1.staticflickr.com/3/2632/32079844734_79b632baa7_n.jpg
   :height: 300px
.. image:: https://c1.staticflickr.com/3/2344/32079474554_9f2fd0ff3b_n.jpg
   :height: 300px

.. image:: https://c1.staticflickr.com/3/2106/32079474524_2f9df5b1a9_n.jpg
   :height: 300px
.. image:: https://c1.staticflickr.com/4/3900/32769057602_dcc944d4a5_n.jpg
   :height: 300px


.. list-table::
   :widths: auto
   :align: center
   :header-rows: 1
   
   * - Name
     - #parameters
     - Dataset
     - Batchsize (single node)
   * - Inception-V3
     - 27M
     - ILSVRC12
     - 32
   * - VGG19
     - 143M
     - ILSVRC12
     - 32
   * - VGG12-22k
     - 229M
     - ILSVRC12
     - 32
   * - ResNet-152
     - 60.2M
     - ILSVRC12
     - 32

For distributed execution, Poseidon consistently delivers near-linear increases in throughput across various models and engines. For example, poseidon registered a 31.5x speedup on training the Inception-V3 network using the TensorFlow (0.10) engine on 32 nodes, which is a 50% improvement upon the original TensorFlow (20x speedup). When training a 229M parameter network (VGG19-22K), Poseidon still achieves near-linear speedup (30x on 32 nodes) using the same TensorFlow (0.10) engine, while distributed TensorFlow sometimes experiences negative scaling with additional machines.

Reference
---------

`Poseidon: A System Architecture for Efficient GPU-based Deep Learning on Multiple Machines <https://arxiv.org/pdf/1512.06216v1.pdf>`_

Contact
-------

* Adam Schwab - adam.schwab@petuum.com
* Hong Wu - hong.wu@petuum.com
* Hao Zhang - hao.zhang@petuum.com

