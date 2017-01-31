# CIFAR-10 Setup Guide
CIFAR-10 is a common benchmark in machine learning for image recognition.

http://www.cs.toronto.edu/~kriz/cifar.html

Code in this directory demonstrates how to use Poseidon to train and evaluate a convolutional neural network (CNN) on both CPU and GPU. We also demonstrate how to train a CNN over multiple GPUs.

Detailed instructions on how to get started available at:

http://tensorflow.org/tutorials/deep_cnn/

Next, we are going to show you how to deploy this sample model on a cluster.

## Data Preparation

### Download
The dataset will be downloaded automatically when you run the training code.

### Partition

## Training
Suppose you are deploying Poseidon on a cluster with *n* nodes, the IP address of the i'th node is *ip[i]*.

First, you need to start a **ps_master** on one node. You may launch it on any node you like. Here we choose the first node. Run the following command on node_0.

```
ps_master tcp://ip[0]:5555 n &
```
A **ps_master** starts on node_0 and listens to port 5555. The second argument is the expected number of **ps_server**s and **worker**s, which equals to the number of nodes in the cluster.

Then, start a **ps_server** on ***EACH*** node. Run the following command on node_i.

```
ps_server tcp://ip[i]:6666 tcp://ip[0]:5555 &
```
The first argument is the port number assigned to the i'th **ps_server**. The second one is the IP address of the **ps_master**.


At last, start a **worker** process on ***EACH*** node by running the following command on node_i for each i.

```
python cifar10_train.py --distributed=True --master_address=tcp://ip[0]:5555 --client_id=i
```

**client_id** must be unique and in the range of [0, n-1].

You may run the above commands by sshing to each node and typing them in manually. Or you can write a simple script to do this. We provide a template [here] for reference.

## Evaluating
Poseidon's evaluating procedure is the same as TensorFlow's.

https://www.tensorflow.org/tutorials/deep_cnn/#evaluating_a_model

## Configuration For Distributed Training
Since Poseidon shares programming interfaces with TensorFlow, you can build up your model in the same way you use TensorFlow. Here is TensorFlow's [api_docs](https://www.tensorflow.org/api_docs/).

The only difference is that we need to provide the cluster configuration to Poseidon when we create a **tf.Session**. Here is an example:

```
config = tf.ConfigProto(log_device_placement = FLAGS.log_device_placement)
if FLAGS.distributed:
	config.distributed = True
	config.master_address = FLAGS.master_address
	config.client_id = FLAGS.client_id
sess = tf.Session(config = config)

```

**config.distributed** indicates whether Poseidon is running in a distributed environment or not. **master_address** is the IP address of the master process. **client_id** is an unique id assigned to each worker.

If there are fully connected layers in your model and you would like to use ***Sufficient Factor Broadcast(SFB)*** to accelerate the training process, you will need to set the **use_sfb** argument to **True**. Then you will need to assign each worker an IP address for its sfb_server. The address list of every sfb_server must be provided to ***EACH*** worker through the **peer_addresses** argument. At last, you will need to provide the size of mini-batches through the **batch_size** argument. Here is an example:

```
config.use_sfb = True
config.peer_addresses.append("tcp://10.117.1.40:7777")
config.peer_addresses.append("tcp://10.117.1.37:7777")
config.batch_size = FLAGS.batch_size
```

Please note that all the workers **MUST** have the same batch size.

Here are some other arguments which are related to communication settings. You may keep the default values by ignoring these arguments if you have no special requirement.

| Arguments | Explanation |
|-----------|-------------|
|num_push_threads|number of threads which pushes parameters from workers to servers, default value is 4|
|num_pull_threads|number of threads which pulls parameters from servers to workers, default value is 8|
|block_size|tensors will be cut into key-value pairs with the size of block_size|

## Training By Using Multiple GPU Cards
***Currently, SFB is NOT supported for multi-gpu training.***

To run multi-gpu training with Poseidon's parameter server, you only need to do the things described above. No more arguments or configuration are needed.