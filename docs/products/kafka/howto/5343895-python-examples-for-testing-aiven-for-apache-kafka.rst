Python examples for testing Aiven for Apache Kafka
==================================================

The sample scripts in this article demonstrate how to connect to your
Aiven for Apache Kafka service and pass a few messages.

.. note:: The examples given here provide different options for the authentication
   method. For more information on the supported methods, see `our article on Kafka
   authentication types <https://developer.aiven.io/docs/products/kafka/concepts/auth-types>`_.

Requirements
------------

-  `kafka-python <https://pypi.org/project/kafka-python/>`__

To test your Aiven for Apache Kafka service:

#. Download the SSL certificate files in the Aiven web console.

   #. Go to the *Overview* page of your Aiven for Apache Kafka service.

   #. Click **Download** next to *Access Key* and save the ``service.key`` file.

   #. Click **Download** next to *Access Certificate* and save the ``service.cert`` file.

   #. | Click **Download** next to *CA Certificate* and save the ``ca.pem`` file.

#. | If you want to use SASL authentication, turn on
     **kafka_authentication_methods.sasl** under *Advanced
     configuration* on the *Overview* page of your service.

#. | Go to the *Topics* page for your service and add a new topic named ``demo-topic`` .

Create and run the producer script
----------------------------------

Using SSL authentication
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

         # This script connects to Kafka and send a few messages

         from kafka import KafkaProducer

         producer = KafkaProducer(
             bootstrap_servers="server-name:port",
             security_protocol="SSL",
             ssl_cafile="ca.pem",
             ssl_certfile="service.cert",
             ssl_keyfile="service.key",
         )

         for i in range(1, 4):
             message = "message number {}".format(i)
             print("Sending: {}".format(message))
             producer.send("demo-topic", message.encode("utf-8"))

         # Force sending of all messages

         producer.flush()

Using SASL-SSL authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

         import kafka
         host = "kafka-customer-demo-exercise1.aivencloud.com"
         sasl_port = 30288
         username = "foo"
         password = "xxx"

         client = kafka.KafkaProducer(
            bootstrap_servers=f"{host}:{sasl_port}",
            sasl_mechanism="PLAIN",
            sasl_plain_password=password,
            sasl_plain_username=username,
            security_protocol="SASL_SSL",
            ssl_cafile="ca.crt",
         )

         client.send("topic1", b"test")
         client.flush()

Using SASL SCRAM authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

         import kafka
         host = "kafka-customer-demo-exercise1.aivencloud.com"
         sasl_port = 30288
         username = "foo"
         password = "xxx"

         client = kafka.KafkaProducer(
            bootstrap_servers=f"{host}:{sasl_port}",
            sasl_mechanism="SCRAM",
            sasl_plain_password=password,
            sasl_plain_username=username,
            security_protocol="SASL_SSL",
            ssl_cafile="ca.crt",
         )

         client.send("topic1", b"test")
         client.flush()

Create the consumer script
--------------------------

Using SSL authentication
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

         # This script receives messages from a Kafka topic

         from kafka import KafkaConsumer

         consumer = KafkaConsumer(
             "demo-topic",
             auto_offset_reset="earliest",
             bootstrap_servers="server-name:port",
             client_id="demo-client-1",
             group_id="demo-group",
             security_protocol="SSL",
             ssl_cafile="ca.pem",
             ssl_certfile="service.cert",
             ssl_keyfile="service.key",
         )

         # Call poll twice. First call will just assign partitions for our
         # consumer without actually returning anything

         for _ in range(2):
             raw_msgs = consumer.poll(timeout_ms=1000)
             for tp, msgs in raw_msgs.items():
                 for msg in msgs:
                     print("Received: {}".format(msg.value))

         # Commit offsets so we won't get the same messages again

         consumer.commit()

Using SASL-SSL authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

         from kafka import KafkaConsumer
         host = "kafka-customer-demo-exercise1.aivencloud.com"
         sasl_port = 30288
         username = "bar"
         password = "xxx"

         client = KafkaConsumer(
            "demo-topic",
            auto_offset_reset = "earliest",
            bootstrap_servers = '{}:{}'.format(host, sasl_port),
            client_id = "demo-client-1",
            group_id = 'demo-group',
            sasl_mechanism = "PLAIN",
            sasl_plain_username = username,
            sasl_plain_password = password,
            security_protocol = "SASL_SSL",
            ssl_cafile = "ca.pem"
         )

         #consumer code

Using SASL SCRAM authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

         from kafka import KafkaConsumer
         host = "kafka-customer-demo-exercise1.aivencloud.com"
         sasl_port = 30288
         username = "bar"
         password = "xxx"

         client = KafkaConsumer(
            "demo-topic",
            auto_offset_reset = "earliest",
            bootstrap_servers = '{}:{}'.format(host, sasl_port),
            client_id = "demo-client-1",
            group_id = 'demo-group',
            sasl_mechanism = "SCRAM",
            sasl_plain_username = username,
            sasl_plain_password = password,
            security_protocol = "SASL_SSL",
            ssl_cafile = "ca.pem"
         )

         #consumer code
