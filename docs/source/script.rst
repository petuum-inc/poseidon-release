Create Poseidon Program From Tensorflow
---------------------------------------

Posiedon worker scripts are almost identical to native TensorFlow, but with an extended session initialization object. In order to demonstrate the changes, we have a native TensorFlow script with a corresponding Poseidon script.

Below is an example TensorFlow script, we'll call it ``mymodel_train.py``:

.. code:: python

    import tensorflow as tf
    import mymodel # Ficticious model

    FLAGS = tf.app.flags.FLAGS

    tf.app.flags.DEFINE_integer('max_steps', 100, 'Loop count')

    def train():
      with tf.device('/gpu:0'):
        model_op = mymodel.train() # Define model
      init = tf.initialize_all_variables()
      sess = tf.Session()
      sess.run(init)
      for step in xrange(FLAGS.max_steps):
        model_out = sess.run(model_op)
        print 'step ' + str(step) + ' ' + model_out

    def main(argv=None):
      train()

    if __name__ == '__main__':
      tf.app.run()

The above could be executed with the following command:

.. code:: bash

    python mymodel_train.py --max_steps 1000

A modified ``mymodel_train.py`` for Poseidon would look like the following:
      
.. code:: python

    import tensorflow as tf
    import mymodel

    FLAGS = tf.app.flags.FLAGS
    
    # Three new command line options
    tf.app.flags.DEFINE_boolean('distributed', False, "Use Poseidon")
    tf.app.flags.DEFINE_string('master_address', "tcp://0.0.0.0:5555", "master address")
    tf.app.flags.DEFINE_integer('client_id', -1, "client id")

    tf.app.flags.DEFINE_integer('max_steps', 100, 'Loop count')

    def train():
      with tf.device('/gpu:0'):
        model_op = mymodel.train()
      init = tf.initialize_all_variables()

      # Add a config variable to pass Poseidon settings
      config = tf.ConfigProto()
      config.master_address = FLAGS.master_address
      config.client_id = FLAGS.client_id
      config.distributed = FLAGS.distributed

      # Initialize tf.Session with the config
      sess = tf.Session(config = config)
      sess.run(init)
      for step in xrange(FLAGS.max_steps):
        model_out = sess.run(model_op)
        print 'step ' + str(step) + ' ' + model_out

    def main(argv=None):
      train()

    if __name__ == '__main__':
      tf.app.run()

Given a config.json file, Poseidon can be executed using the above script with the command below:

.. code:: bash

    psd_run -c config.json -o ~/logs "python mymodel_train.py --max_steps 1000"

Note: ``psd_run`` adds the following flags to the worker when it launches:

* distributed
* master_address
* client_id

