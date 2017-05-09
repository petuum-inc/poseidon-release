Create Poseidon Program From TensorFlow
---------------------------------------

Posiedon worker scripts are almost identical to native TensorFlow, but with an extended session initialization object. In order to demonstrate the changes, we have a native TensorFlow script with a corresponding Poseidon script.

Below is an example TensorFlow script, we'll call it ``mymodel_train.py``:

.. code:: python

    import tensorflow as tf

    FLAGS = tf.app.flags.FLAGS

    tf.app.flags.DEFINE_integer('max_steps', 5, 'Loop count')

    def model():
      x1 = tf.Variable(1.5, name='x1')
      x2 = tf.Variable(3.5, name='x2')
      out = x1 + 2 * x2
      grads = tf.gradients(ys=[out], xs=[x1, x2])
      return grads

    def train():
      model_op = model()
      init = tf.initialize_all_variables()
      sess = tf.Session()
      sess.run(init)
      for step in xrange(FLAGS.max_steps):
        model_out = sess.run(model_op)
        print 'step ' + str(step) + ' ' + str(model_out)

    def main(argv=None):
      train()

    if __name__ == '__main__':
      tf.app.run()

The above could be executed with the following command:

.. code:: bash

    python mymodel_train.py --max_steps 10

A modified ``mymodel_train.py`` for Poseidon will have three changes.

1. Add three commandline arguments - distributed, master_address and client_id:

.. code:: python

    tf.app.flags.DEFINE_boolean('distributed', False, "Use Poseidon")
    tf.app.flags.DEFINE_string('master_address', "tcp://0.0.0.0:5555", "master address")
    tf.app.flags.DEFINE_integer('client_id', -1, "client id")
    
2. Build a tf.ConfigProto() using these commandline args:

.. code:: python

    config = tf.ConfigProto()
    config.master_address = FLAGS.master_address
    config.client_id = FLAGS.client_id
    config.distributed = FLAGS.distributed

3. Call tf.Session with the new config object:

.. code:: python

    sess = tf.Session(config = config)

Putting the three changes together, we get the script below.

.. code:: python

    import tensorflow as tf

    FLAGS = tf.app.flags.FLAGS
    
    # Three new command line options
    tf.app.flags.DEFINE_boolean('distributed', False, "Use Poseidon")
    tf.app.flags.DEFINE_string('master_address', "tcp://0.0.0.0:5555", "master address")
    tf.app.flags.DEFINE_integer('client_id', -1, "client id")

    tf.app.flags.DEFINE_integer('max_steps', 5, 'Loop count')
    
    def model():
      x1 = tf.Variable(1.5, name='x1')
      x2 = tf.Variable(3.5, name='x2')
      out = x1 + 2 * x2
      grads = tf.gradients(ys=[out], xs=[x1, x2])
      return grads

    def train():
      model_op = model()
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
        print 'step ' + str(step) + ' ' + str(model_out)

    def main(argv=None):
      train()

    if __name__ == '__main__':
      tf.app.run()

Given a config.json file, Poseidon can be executed using the above script with the following command (remember to change /path/to/mymodel_train.py):

.. code:: bash

    psd_run -c config.json -o ~/logs "python /path/to/mymodel_train.py --max_steps 10"

Note: ``psd_run`` adds the following flags to the worker when it launches:

* distributed
* master_address
* client_id

