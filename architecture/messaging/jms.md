# Message brokers

- translates a message from the formal messaging protocol of the sender to the formal messaging protocol of the receiver
- used for communication between applications by exchanging messages (formally-defined)
- route messages to one or more destinations
- transform messages
- perform aggregation, decomposition of messages
- can store messages into db
- respond to events

## Messaging Protocols

Three of them are most widely used:

1. STOMP - Simple Text-Oriented Messaging Protocol
    - doesn't deal with concepts like queues and topics
    - uses a SEND semantic with a "destination" string for where 
    the message deliver
    - the receiver can implement queues, topics and exchanges
    - http://stomp.github.io
    
2. MQTT - Message Queue Telemetry Transport
    - M2M (Machine to Machine) / Internet of Things connectivity protocol
    - simply publish-subscribe messaging
    - support thousands of concurrent device connections
    
3. AMQP - Advanced Message Queuing Protocol
    - different implementation of AMQP protocol will work interoperable
    - use cases:
        - deliver messages when the destination comes online
        - encrypted assured transactions
        - real time feed of constantly updating information

### Implementations:

* RabbitMQ
* ActiveMQ