This is a quick tutorial to run a distributed poseidon task on both CPU and GPU. In this tutorial, we will use [CIFAR-10](http://www.cs.toronto.edu/~kriz/cifar.html), which is a common benchmark in machine learning for image recognition using convolutional neural network(CNN). More detailed instructions on how to get started available at: [https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/](https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/).

## Data Downloading
The dataset will be downloaded automatically when you run the training code.

## Training

First, make sure you have Poseidon successfully installed in your cluster. Follow the [User Installation](http://poseidon-release.readthedocs.io/en/latest/User_Installation/#cluster-installation) section for instruction.

Now, suppose you have Poseidon installed at `/usr/local/lib/python2.7/dist-packages/tensorflow`, we will call it `$POSEIDON_HOME` below.(Here we keep the namespace `tensorflow` to minimize the modification effort.) The main entry to run Poseidon tasks is `psd_run`.
```
Usage: 
  psd_run options args

Example:
  psd_run -c(--cluster_config) config.json "python conv.py --batch 10"

Options:
  -h, --help            show this help message and exit
  -c CLUSTER_CONFIG, --cluster_config=CLUSTER_CONFIG
                       configuration file for cluster environment: specify
                       workers in each machine.
                       See example:  {
                       "worker_nodes": [
                           "192.168.1.11",
                           "192.168.1.12",
                           "192.168.1.15"],
                       "server_nodes": [
                           "192.168.1.70",
                           "192.168.1.80",
                           "192.168.1.90"]
                       }
  -o OUTPUT_FOLDER, --out=OUTPUT_FOLDER
                       output log folder
```
For example, you need to write a json format file `aws_cluster.json` including `worker_nodes` and `server_nodes` attributes. For current release, the number of worker nodes must be the same with the number of server_nodes. After that, you can start a CIFAR-10 task like this: `psd_run -c aws_cluster.json "python /home/ubuntu/cifar10_train.py --max_steps 100"`. You can download the training script `cifar10_train.py` from [here](https://raw.githubusercontent.com/petuum-inc/poseidon-release/master/models/cifar10/cifar10_train.py). Make sure giving the absolute path here.

## Poseidon Logs
After running Poseidon, you can check the execution log `poseidon_run.log` in the same path you run Poseidon. There are also addtional log files for debugging and monitoring purpose created in `poseidon_log_$TIMESTAMP_SUFFIX` folder.

## Evaluating
Poseidon's evaluating procedure is the same as TensorFlow's. Please follow the tutorial [here](https://www.tensorflow.org/versions/r0.10/tutorials/deep_cnn/#evaluating_a_model).

## Configuration For Distributed Training
Since Poseidon shares programming interfaces with TensorFlow, you can build up your model in the same way you use TensorFlow. Here is TensorFlow's [api_docs](https://www.tensorflow.org/versions/r0.10/api_docs/python/). The only difference is that you need to provide the cluster configuration to Poseidon when we create a **tf.Session**. Here is an example:

```
config = tf.ConfigProto(log_device_placement = FLAGS.log_device_placement)
config.master_address = FLAGS.master_address
config.client_id = FLAGS.client_id
config.num_push_threads = 1
config.num_pull_threads = 1
sess = tf.Session(config = config)

```

**master_address** is the IP address of the master process. **client_id** is an unique id assigned to each worker.

If there are fully connected layers in your model and you would like to use ***Sufficient Factor Broadcast(SFB)*** to accelerate the training process, you will need to set the **use_sfb** argument to **True**. Then you will need to assign each worker an IP address for its sfb_server. The address list of every sfb_server must be provided to ***EACH*** worker through the **peer_addresses** argument. At last, you will need to provide the size of mini-batches through the **batch_size** argument. Here is an example:

```
config.use_sfb = True
config.peer_addresses.append("tcp://10.117.1.40:7777")
config.peer_addresses.append("tcp://10.117.1.37:7777")
config.batch_size = FLAGS.batch_size
```

Please note that all the workers **MUST** have the same batch size.

Here are some other advanced arguments which are related to communication settings. You may keep the default values by ignoring these arguments if you have no special requirement.

| Arguments | Explanation |
|-----------|-------------|
|num_push_threads|number of threads which pushes parameters from workers to servers, default value is 4|
|num_pull_threads|number of threads which pulls parameters from servers to workers, default value is 8|
|block_size|tensors will be cut into key-value pairs with the size of block_size|

## Training By Using Multiple GPU Cards
***Currently, SFB is NOT supported for multi-gpu training.***

To run multi-gpu training with Poseidon's parameter server, you only need to do the things described above. No more arguments or configuration are needed.
