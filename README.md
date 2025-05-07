>What is amqp?

AMQP (Advanced Message Queuing Protocol) is an open standard application layer protocol designed for message-oriented middleware. It is an open, vendor-neutral application-layer protocol designed for asynchronous, reliable messaging between distributed applications. It defines both the network wire-level protocol and the broker-mediated messaging model so that different clients and brokers can interoperate seamlessly.

>What does it mean? guest:guest@localhost:5672 , what is the first guest, and what
is the second guest, and what is localhost:5672 is for?

`amqp://guest:guest@localhost:5672` is the standard AMQP “connection string” format. 

- `guest:guest`. The first `guest` is the username that the user is authenticating as. The second `guest` is that user’s password.
- `localhost:5672`. `localhost` is the hostname. `5672` is the default TCP port that RabbitMQ (and other AMQP brokers) listen on for client connections.

The code `CrosstownBus::new_queue_listener("amqp://guest:guest@localhost:5672".to_owned())` means: Connect to the broker running on my local machine (port 5672), and log in using username `guest` and password `guest`.

## RabbitMQ

### Simulation slow subscriber

![sschart](images/sschart.png)

n the image above, I intentionally slowed down the Subscriber's ability to receive or process data from the message broker by adding a 1-second delay to each operation. As a result, the number of queued messages in the message broker increased over time because the Publisher was sending data faster than the Subscriber could receive it. In my case, the total number of queued messages in the message broker reached 20 after running the Publisher twice.

### Reflection and Running at least three
subscribers

![ssconsolemanysubs](images/ssconsolemanysubs.png)

![sschartmanysubs](images/sschartmanysubs.png)

I did the same experiment as in the "Simulation of a Slow Subscriber" section. However, this time, I ran multiple Subscribers. As a result, the data transmission became much faster, and in my case, I noticed that no new messages were being added to the queued messages in the message broker. This happens because the message broker distributes the incoming data from the Publisher to all connected Subscribers, ensuring efficient processing.

As we can see, after opening three Subscribers, I observed a significant improvement in message processing. My queued messages, which initially stood at 20, were reduced to just 3, demonstrating how increasing the number of Subscribers can drastically improve throughput and decrease backlog in the message broker.

Without modifying the program's code, we can achieve different results simply by adjusting the message broker's configuration or the number of active Subscribers. This concept is known as event-driven architecture.

