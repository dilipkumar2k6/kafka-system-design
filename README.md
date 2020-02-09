# Step1
## Collect functional requirement 
- publish message to system, multiple publisher
- Consume message from system, multiple consumer
    - Consume message for given message Id
- Do not loose message, persistence
- Retention
- Ordering gurantee
- Realtime performance
- Throughput
- Resending failed messages exactly once or at least once semantics

## Collect design constraints

# Step 2
## Micro services
- A single service
- App server tier just to serve stats
- Everything happened in protocol therefore no app server is needed here

# Step 3
## Algorithm
Data model: 
MessageId as key: Message body as value V
### How to store in persistence layer?
- Store messages into append only log, and then keep and index of <messageId, location in log>
- Retention period by default 7 days
- Log emulates a ring buffer
- Kafka uses skip list
- Since there is no whole so you can call for truncate on file for given range for cleanup
### Skip list
![](skip-list.png)
### How to manage sorted order?
- Producer have to maintain the sequence number
## Why distributed?
1. Storage
2. Throughput
3. Availability
4. Geo location
5. Latency
## Distributed system
### Sharding
- K: V : Body storing as a single log
- Kafka does horizontal sharding
    - <Topic, Partition within a topic, msgid >, body
    - Every topic and partition become a separate log
    - create_topic(Wimbledon, 1024)
    - send(Wimbledon, partitionId, message body)
- Commit logic. Consumer keep track of what they consumed. It keep trak of messageID. If consumer dies then consumer will put needle back to messageId
- 
