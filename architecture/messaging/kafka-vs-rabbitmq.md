# Apache Kafka vs RabbitMQ comparison

https://www.cloudamqp.com/blog/2019-12-12-when-to-use-rabbitmq-or-apache-kafka.html


RabbitMQ is a solid, general-purpose message broker that supports several protocols such as AMQP, MQTT, STOMP, etc. It can handle high throughput. A common use case for RabbitMQ is to handle background jobs or long-running task, such as file scaning, image scaling or PDF conversion. RabbitMQ is also used between microservices, where it serves as a means of communicating between applications, avoiding bottlenecks passing messages.

Kafka is a message bus optimized for high-throughput ingestion data streams and replay. Use Kafka when you have the need to move a large amount of data, process data in real-time or analyze data over a time period. In other words, where data need to be collected, stored, and handled. An example is when you want to track user activity on a webshop and generate suggested items to buy. Another example is data analysis for tracking, ingestion, logging or security.

Kafka can be seen as a durable message broker where applications can process and re-process streamed data on disk. Kafka has a very simple routing approach. RabbitMQ has better options if you need to route your messages in complex ways to your consumers. Use Kafka if you need to support batch consumers that could be offline or consumers that want messages at low latency. 

In order to understand how to read data from Kafka, we first need to understand its consumers and consumer groups. Partitions allow you to parallelize a topic by splitting the data across multiple nodes. Each record in a partition is assigned and identified by its unique offset. This offset points to the record in a partition. In the latest version of Kafka, Kafka maintains a numerical offset for each record in a partition. A consumer in Kafka can either automatically commit offsets periodically, or it can choose to control this committed position manually. RabbitMQ will keep all states about consumed/acknowledged/unacknowledged messages. I find Kafka more complex to understand than the case of RabbitMQ, where the message is simply removed from the queue once it's acked.

RabbitMQ's queues are fastest when they're empty, while Kafka retains large amounts of data with very little overhead - Kafka is designed for holding and distributing large volumes of messages. (If you plan to have very long queues in RabbitMQ you could have a look at lazy queues.)

Kafka is built from the ground up with horizontal scaling (scale by adding more machines) in mind, while RabbitMQ is mostly designed for vertical scaling (scale by adding more power).

RabbitMQ has a built-in user-friendly interface that lets you monitor and handle your RabbitMQ server from a web browser. Among other things, queues, connections, channels, exchanges, users and user permissions can be handled - created, deleted and listed in the browser and you can monitor message rates and send/receive messages manually. Kafka has a number of open-source tools, and also some commercial once, offering the administration and monitoring functionalities. I would say that it's easier/gets faster to get a good understanding of RabbitMQ.

In general, if you want a simple/traditional pub-sub message broker, the obvious choice is RabbitMQ, as it will most probably scale more than you will ever need it to scale. I would have chosen RabbitMQ if my requirements were simple enough to deal with system communication through channels/queues, and where retention and streaming is not a requirement.

There are two main situations where I would choose RabbitMQ; For long-running tasks, when I need to run reliable background jobs. And for communication and integration within, and between applications, i.e as middleman between microservices; where a system simply needs to notify another part of the system to start to work on a task, like ordering handling in a webshop (order placed, update order status, send order, payment, etc.).

In general, if you want a framework for storing, reading (re-reading), and analyzing streaming data, use Apache Kafka. It’s ideal for systems that are audited or those that need to store messages permanently. These can also be broken down into two main use cases for analyzing data (tracking, ingestion, logging, security etc.) or real-time processing.

More reading, use cases and some comparison data can be found here: https://www.cloudamqp.com/blog/2019-12-12-when-to-use-rabbitmq-or-apache-kafka.html

Also recommending the industry paper: "Kafka versus RabbitMQ: A comparative study of two industry reference publish/subscribe implementations": http://dl.acm.org/citation.cfm?id=3093908

I do work at a company providing both Apache Kafka and RabbitMQ as a Service.