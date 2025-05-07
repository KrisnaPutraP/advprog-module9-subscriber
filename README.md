>What is amqp?

AMQP (Advanced Message Queuing Protocol) is an open standard application layer protocol designed for message-oriented middleware. It is an open, vendor-neutral application-layer protocol designed for asynchronous, reliable messaging between distributed applications. It defines both the network wire-level protocol and the broker-mediated messaging model so that different clients and brokers can interoperate seamlessly.

>What does it mean? guest:guest@localhost:5672 , what is the first guest, and what
is the second guest, and what is localhost:5672 is for?

`amqp://guest:guest@localhost:5672` is the standard AMQP “connection string” format. 

- `guest:guest`. The first `guest` is the username that the user is authenticating as. The second `guest` is that user’s password.
- `localhost:5672`. `localhost` is the hostname. `5672` is the default TCP port that RabbitMQ (and other AMQP brokers) listen on for client connections.

The code `CrosstownBus::new_queue_listener("amqp://guest:guest@localhost:5672".to_owned())` means: Connect to the broker running on my local machine (port 5672), and log in using username `guest` and password `guest`.