# Designing applications

Based on book - Designing Data Intensive Applications

### The main goals for data intensive apps:

1. Reliability

    - continuing to work correctly, even when things go wrong (faults)
    - system that anticipate faults and can cope with them are called
    fault-tolerant or resilient
    - failure is when the system as w whole stops providing the required
    service
    - therefore the goal is to design fault tolerant system that prevent
    faults from causing failures
    - faults:
        - hardware crashes
        - software errors
        - human errors
        
2. Scalability

    - system has ability to cope with increased load
    - load parameters examples:
        - requests per second to a web server
        - the ratio of reads and writes in a database
        - the number of simultaneously active users
        - it depends on application characteristics
    - describing performance:
        - throughput - the number of records we can process per second
        - latency and response time
            - latency is a duration that a request is waiting to be
            handled (awaiting for service)
            - response time - the actual time to process the request 
            (service time) plus network delays and queueing delays
            - good metric for response time is median (percentiles 
            not average time), median is the 50th percentile, to see how
            bad it looks you can look at higher percentiles: the 95th,
            99th
    - scaling types:
        - scaling up (vertical scaling) - moving to a more powerful 
        machine
        - scaling out (horizontal scaling) - distributing the load across
        multiple smaller machines

3. Maintainability