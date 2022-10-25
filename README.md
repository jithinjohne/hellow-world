# hellow-world
My first repository
Hi,
This is Jithin experimenting with GitHub.


        private static void VirtualQueue()
        {
            IConnectionFactory connectionFactory = new NMSConnectionFactory(_config.BrokerUri);
            //String brokerURI = ActiveMQConnectionFactory.DEFAULT_BROKER_URL;

            //ConnectionFactory connectionFactory =
            //  new ActiveMQConnectionFactory(brokerURI);
            var consumerConnection = connectionFactory.CreateConnection();
            consumerConnection.Start();

            //Create the first consumer for Consumer.Foo.VirtualTopic.orders
            var sessionA = consumerConnection.CreateSession(AcknowledgementMode.AutoAcknowledge);
            var fooQueueA = sessionA.GetQueue("Consumer.A.VirtualTopic.orders");
            var consumerA = sessionA.CreateConsumer(fooQueueA);
            consumerA.Listener += OnMessageReceivedA;

            // Create the second consumer for Consumer.Foo.VirtualTopic.orders
            var sessionB = consumerConnection.CreateSession(AcknowledgementMode.AutoAcknowledge);
            var fooQueueB = sessionB.GetQueue("Consumer.B.VirtualTopic.orders");
            var consumerB = sessionB.CreateConsumer(fooQueueB);
            consumerB.Listener += OnMessageReceivedB;

            // Create the sender
            String topicName = "VirtualTopic.orders";
            var senderConnection = connectionFactory.CreateConnection();
            senderConnection.Start();
            var senderSession = senderConnection.CreateSession(AcknowledgementMode.AutoAcknowledge);
            var ordersDestination = senderSession.GetTopic(topicName);
            var producer = senderSession.CreateProducer(ordersDestination);
            producer.DeliveryMode = MsgDeliveryMode.Persistent;

            IMessage message = senderSession.CreateTextMessage("Hello");
            producer.Send(message);
        }

        private static void OnMessageReceivedB(IMessage message)
        {
            Console.WriteLine("Message received on B");
        }

        private static void OnMessageReceivedA(IMessage message)
        {
            Console.WriteLine("Message received A ");
        }
