Please refer to this [tutorial](https://www.tensorflow.org/tutorials/deep_cnn/) for some information about Convolutional Neural Networks(CNNs) with TensorFlow and the CIFAR-10 dataset.

Next, we are going to help you run a sample CNN model. You can get the codes from [here]. After that, we will show you how to customize your own model.

## Data Preparation

- Download
The dataset will be downloaded automatically when you run the training code.

- Partition

## Training
Suppose you are deploying Poseidon on a cluster with *n* nodes, the IP address of the i'th node is *ip[i]*.

First, start a **ps_master** on any node. Here we start the **ps_master** on the first node. Run the following command on node_0.

```
ps_master tcp://ip[0]:5555 n &
```

Then, start a **ps_server** by running the following command on ***EACH*** node.

```
ps_server tcp://ip[i]:6666 tcp://ip[0]:5555 &
```

At last, start a worker process on ***EACH*** node.

```
python cifar10_train.py --distributed=True --master_address=tcp://ip[0]:5555 --client_id=i
```

**client_id** must be unique and in the range of [0, n-1].

You may run the above commands by sshing to each node and typing them in manually. Or you can write a simple script to do this. We provide a template [here] for reference.

## Evaluating
Poseidon's evaluating procedure is the same as TensorFlow's.

https://www.tensorflow.org/tutorials/deep_cnn/#evaluating_a_model

## Customizing Your Own Model
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
