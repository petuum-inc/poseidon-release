
Welcome to Poseidon
===================

Poseidon is a highly scalable and efficient system architecture for large-scale deep learning.

This distribution of Poseidon uses the `Tensorflow 0.10 client API <https://www.tensorflow.org/versions/r0.10/>`_.

Introduction
------------

The primary purpose of Poseidon is to allow deep learning applications written and tested on single GPU nodes to easily scale onto a cluster environment. The techniques present in Poseidon could be used to produce a better distributed version for any existing deep learning framework. This release includes Poseidon TensorFlow 0.10, a more efficient, scalable distributed deep learning system than native TensorFlow.

Contents
--------
.. toctree::
   :maxdepth: 4

   install
   tutorial
   troubleshoot

Performance at a Glance
-----------------------

Poseidon can scale almost linearly in total throughput with additional machines while simultaneously incurring little additional overhead.

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

For distributed execution, Poseidon consistently delivers near-linear increases in throughput across various models and engines. For example, poseidon registered a 31.5x speedup on training the Inception-V3 network on 32 nodes, which is a 50% improvement upon the original TensorFlow (20x speedup). When training a 229M parameter network (VGG19-22K), Poseidon still achieves near-linear speedup (30x on 32 nodes), while distributed TensorFlow sometimes experiences negative scaling with additional machines.

Contact
-------

* Adam Schwab - adam.schwab@petuum.com
* Hong Wu - hong.wu@petuum.com
* Hao Zhang - hao.zhang@petuum.com

