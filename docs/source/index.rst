
Welcome to Poseidon
===================

Poseidon is an easy-to-use and efficient system architecture for large-scale deep learning.

This distribution of Poseidon uses the `Tensorflow 1.0.1 client API <https://www.tensorflow.org/versions/r1.0/api_docs/python/>`_. Poseidon can distribute python model scripts designed to run on a single node into a cluster with no changes to the scripts themselves. This allows for rapid prototyping of model scripts. Developers can focus on perfecting their models running on a single node and be reasonably confident that the model will run efficiently scaled to 32 nodes or more.

Introduction
------------

Poseidon allows deep learning applications written in popular languages and tested on single GPU nodes to easily scale onto a cluster environment with high performance, correctness, and low resource usage. This release runs the TensorFlow 1.0.1 api on distributed GPU clusters - greatly improving convenience, efficiency and scalability over the standard opensource TensorFlow software.

Contents
--------
.. toctree::
   :maxdepth: 4

   install
   tutorial
   reference
   troubleshoot

Performance at a Glance
-----------------------

Poseidon can scale almost linearly in total throughput with additional machines while simultaneously incurring little additional overhead.

Contact
-------

* Adam Schwab - adam.schwab@petuum.com
* Hong Wu - hong.wu@petuum.com
* Hao Zhang - hao.zhang@petuum.com

