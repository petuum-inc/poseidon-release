For installation, see the `installation instructions <../install/#installation-options>`_.

*****************
Poseidon Tutorial
*****************

Poseidon is designed to be able to run on a variety of different clusters. This tutorial contains two cluster examples: AWS and a private cluster.

In this tutorial, we will use `CIFAR-10 <http://www.cs.toronto.edu/~kriz/cifar.html>`_, which is a common benchmark in machine learning for image recognition using convolutional neural networks (CNN). The following url provides a description of TensorFlow's implementation: https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/.

Download the Model Script
-------------------------

The model we will use for Cifar10 is created by the TensorFlow team. Clone the git repository for each node you will use for Poseidon:

.. code:: bash
    
    git clone https://github.com/tensorflow/models.git

For reference later, save the model directory that you just cloned into ``$TF_MODEL_HOME`` for reference later:

.. code:: bash

    export TF_MODEL_HOME="`pwd`/models"

The script is designed to download Cifar10 automatically. To test everything is installed correctly (and download the data), we can run cifar10_train.py directly: ``python $TF_MODEL_HOME/tutorial/image/cifar10/cifar10_train.py``

Data
----

Once you have tested cifar10_train.py with the above call, the data should have downloaded to ``/tmp/cifar10_data``. Copy this data onto each node you wish to run Poseidon on to skip the download step during Poseidon execution.

Launch Poseidon
---------------

The first tutorial covers configurations specific to Amazon Web Service EC2 service. The second describes configurations when running on a local network.

.. toctree::
   :maxdepth: 2

   tutorial-aws
   tutorial-private

Evaluating
----------

Poseidon's evaluating procedure is the same as TensorFlow's. Run the evaluation script:

.. code:: bash

    python $TF_MODEL_HOME/tutorial/image/cifar10/cifar10_eval.py
