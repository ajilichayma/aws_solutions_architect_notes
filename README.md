# aws_solutions_architect_notes
## Databases:
- RDS: relational databases services is for online transaction processing (SQL, MySQL, PostgreSQL, Aurora, MariaDB, Oracle)
- RDS features key: 
    - multi AZ: for disaster recovery when we lose the database in the first AZ ( the primary database ), amazon will update the DNS with the new IP address of the database in the second AZ. 
    - read replicas:for performance that means the database is automatically updated ( after modifying the primary database ) and also it is used when we have so many users accessing the database we devise them into parts ; some access the primary, the other part access the secondary and so on.
- RDS runs on virtual machines
- you can’t access these O.S
- RDS is not serverless but aurora is serverless 
- Elasticache used to speed up the performance of existing databases
### Buckups types
- Automated backups: allow you to recover your database to any point in time within a retention period and the retention period can be between one and 35 days.
- Database snapshots:
    - are done manually.
    -  
## EBS:
- instance storage attached, the data will be lost if the instance is terminated.
- we can dettach an EBS from one instance and attach it to another.
- we can backup data on the EBS volumes to S3 by taking point-in-time snapshots.
- we can use Amazon Data LifeCycle  Manager to automate the creation, the retention, and the deletion of snapshots taken to back up the EBS.
- we can share unencrypted/encrypted (only if they are encrypted with a customer managed key) EBS snapshots.
- we can't share encrypted snapshots publicly. 
- we can't share encrypted EBS volumes.
- ### sharing encrypted EBS volume:
  we can't directly share EBS encrypted volume, Instead we can create and share encrypted EBS snapshots.
- ### How to encrypt root device volume:
  1. Provision an EC2 instance
  2. make a snapshot of the root device volume
  3. make a cppy of the snapshot
  4. encrypt the copy
- ### EBS-optimized :
  uses an optimized configuration stack and provides additional, dedicated capacity for amazon EBS I/O. this optimization provides a best performance for EBS by 
  minimizing contention between EBS I/O and other traffic.
- ### EBS encrytion: 
  when creating an encrypted EBS volume, the following types of date will be encrypted
  - data at rest
  - all the data moving between the volume and the instance 
  - all snapshots created from the volume
  - all volumes created from these snapshots 
- ### changing the data's encrytion state: (encrypt unencrypted volume)
  - we can't remove encryption from an encrypted volume 
  - we can't directly encrypt an uncrypted volume, instead we can create an instance with an encrypted root device volume from an encrpted snapshot
  
## Instance store
- we can specify an instance store for an EC2 instance only when launching it
- data in the instance store persists only when we reboot the EC2 instance, they will be lost if:
    - the instance is stoped or terminated
    - the hard disk driver fails
    
## Route 53: is the DNS service for Amazon
- DNS is on the port 53 that's why we got the name of route 53
- DNS is used to convert human friendly names into an internet protocol (IP) addresses 
- IP addresses:
    - IPV4: has a space of 32 bits => we have 4 billion different addresses
    - IPV6: has a space of 128 bits => we have 340 undecillion address 
- The start of authority record stores information about:
    - the name of the server that supplied the data for the zone.
    - the administrator for the zone 
    - the current version of the data file
    - the default nb of seconds for time to live (TTL) file on ressource records.
- TTL: the length that a DNS record is cached on either the resolving server or the users own local pc. it is eqaul to the value of 'time to live' in seconds.
       the lower the TTL, the faster changes to DNS records take to propagate throughout the internet.
- ### Record types:
    - A record: 
    stands for address record, it maps a name to one or more IP addresses.
    - CNAME record: 
    a canonical name, can be used to resolve one domain name to another.
    - Alias record: 
    can be used to resolve one domain name to another
    - AAAA record:
    is similar to an A record but it is for IPv6 addresses (whereas A record is for IPv4).
    -  MX records (Mail Exchange records):
    is used for setting up Email servers. MX records must be mapped correctly to deliver email to your address.

    - The CNAME can't be used for naked domain names (naked domain= base= bare= root apex), howver the alias record can.
    - given the choice, choose an alias record over a CNAME
    - The A record must resolve to an IP. The CNAME and ALIAS records must point to a name.
- ### aws routing policies
    1. simple routing
    2. weighted routig
    3. latency-based routing 
    4. failover routing 
    5. geolocation routing 
    6. geoproximity routing 
    7. multivalue routing 
### 1. simple routing
- you can have only one record with multiple IP addr. If you specifyu multiple values in a record, route 53 returns all values to the user in a random order.
### 2. weighted routig
- allows you to split your traffic based on different weights assigned (different regions) 
- ex: you can set 10% of your traffic to go to us-east-1 and 20% to go to eu-west-1
### 3. latency-based routing 
 - allows you to route your traffic based on the lowest network latency for your end user (ie which region will give you the fasteest response time)
### 4. failover routing 
 - are used when you want to create an active/passive set up.
 - ex: you may want your primary site to be in eu-west-2 and your secondary DR site to be in AP-southeast-2.
 - route 53 will monitor the health of your primary site using a health check ( the health check monitors the health of your endpoints)
### 5. geolocation routing 
- lets you choose where your traffic will be sent based on the geographic location of your users(ie the location from which DNS queries originate)
### 6. geoproximity routing 
- lets amazon route 53 route traffic to your ressources based on the geographic location of your users and your ressources.
- you cn optionally choose to route more or less traffic to a given ressource by specifying z value known as bias.
- to use geoproximity routing, you must use route53 traffic flow.
### 7. multivalue routing 
- lets you configure amazon route53 to return multiple values such as ip adrs for your web server, in response to a dns query.
- you can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each ressource.
- it is similar to simple routing but it allows oyu to put health checks on each record set.

## VPC (virtual private cloud)
- we can have 5 VPCs per region
### Bastion host
is a server whose purpose is to provide access to a private network from an external network such as Internet

### Nat gateway 
- when we have NAT gateway attached to the subnet where web server ec2 resides, the traffic from the internet cannot reach the ec2.
- nat gatewa only routes traffic from aws ressources within a vpc to the internet.
- any traffic from the internet into vpc ressouces is not allowed.
- NAT Gateways don't have security groups
- we can't share NAT gateways across VPCs
- Provides better availability, higher bandwidth, and requires less administrative effort than NAT instances

### Security group  
- are stateful 
- can be understood as a firewal to protect EC2 instances
- in the default security group, all inboud and outbound traffic are allowed by default.
- we can create a custom security group, and by default it allows no inbound traffic and allows all outbound traffic.
- You cannot block specific IP addresses using Security Groups, instead use Network Access Control Lists.
- You can specify allow rules, but not deny rules.

### Network ACL  
- are stateless: any change applied to the incoming rule isn't automatically applied to the outgoing rule.
- can be understood as a firewall for the whole subnet.
- rules are organised with chronological order; the rule n° 100 will trump the rule n°400
=> if we have a deny rule, it must be before the allow rule
- NACL will be evaluated before the security group.
- when creating  VPC, it will be associated with NACL and if we we create subnets, they will be associated by default to the default NACL of the VPC.
but one subnet can be associated with one NACL at one time.
- The default NACL of the VPC allows all outbound and inbound traffic.
- When creating a custom network ACL. By default, all inbound and outbound traffic are denied.

### Direct connect gateway
- It is a gloabally available ressource to enable connections between amazon VPCs across different regions or AWS.
### VPC endpoints
- It enables connections between VPC and supported services without requiring that you use an internet an internet gateway, NAT device, VPN connection or aws direct connect connection.
### VPN
- It uses IPsec to establish encrypted connectivity between your intranet and amazon VPC and the public internet.
### AWS transit gateway
- It uses to interconnect the VPC and on-premises networks.

## SQS (Simple Queue Service)
- it a web service that guves you access to message queue which is used to store messages while waiting for a computer to process them.
- using SQS, you can decouple the components of an app, so they run independtly.
- messages can contain up to 256KB of text in any format. any component can later retreive the messages programmatically using the amazon SQS API.
- a queue is a temporary repository for meassages that are waiting processing
### Queue types
    - Standard queues (default)
        - unlimited throughput: it supports a nearly unlimited nb of transactions per sec.
        - Best-effort ordering: occasionally, msgs might be delevired in an order different from which they were sent.
        - At least one delivery: a msg is delivered at least one, but occasionally more than one copy of a msg is delivered.
    - FIFO queues 
        - High throughput: it supports up to 300 msgs per sec(send, receiveor delete operations per second)
        - First In First Out delivery: this order is strictly preserved.
        - Exactly-one processing: a msg is delivered once and remains available until a consumer process and deletes it. Duplicates are not intoduced in the queue.
### Dead letter Queue
- the main task of a dead letter is handling message failure.
- it lets you set aside and isolate a msg that can't be processed correctly to determine why their processing didn't succeed.
### Delay Queue
- it is the period of time during it the msg remains invisible and can't be executed.
- it can be from 0 up to 12hours, the default timeout is set to 30 seconds.
- after the delay period has been expired, the msg becomes visible & can be executed.
- immediately after a msg is received, it remains in the queue. to prevent other consumers from processing the msg again, Amazon SQS sets a visibility timeout to prevent other consumers from receiving & processing the msg.
### SQS long polling vs SQS short polling 
- SQS long polling is a way to retrieve messagages from SQS queues- waits for the messages to arrive.
- SQS short polling returns immediately ( even if the message queue is empty) 
- SQS long polling can reduce costs.
- SQS long polling can be enabled at the queue level or at the API level using WaitTimeSeconds. 
### SQS remarks
- any question about decoupling the infrastructure, think about SQS

## SNS (Simple Notification Service)
- it is a web service that makes it easy to set up, operate, and send notifications from the cloud.
- it is a publish/subscriber syst
- it provides developers with a highly scalable, flexible, and cost-effective capacity to publish messages from an app and immediately deliver them to subscribers or other apps.
- it allows you to group multiple recipients using topics.
- to prevent messages from being lost, all messages published to Amazon SNS are stored redundantly across multiple AZ.
- each meassage attribute consists of: a name, a type and a value.
### SNS Benefits 
- push based delivery 
- simple & easy to integrate 
- pay as you go model with no up front costs.
## SWF (Simple Workflow Service)
- it has a way of combining the digital environment with manual tasks ( with human being ) .remember the example of the book order
## API gateway 
## Kinesis 
- It is a managed alternative to Apache Kafka 
- great for application logs, metrics, IoT, clickstreams
- Great for real-time big data
- Great for streaming processing frameworks.
- Data is automatically replicated synchronously to 3 AZ.
### Kinesis types
- Kinesis Streams: low latency streaming ingest 
    - Streams are devided into ordered shards(partitions)
    - Data retention is 24h by default, can go up to 7 days 
    - Ability to reprocess/replay data: once you consume the data, the data is not gone from kinesis, it will be gone from the data retention period but you can reuse it               over and over
     - multiple applications can consume the same stream => that means it provides real time processing with scale of throughput
     - once you isert data into kinesis, it can't be deleted ( it is immutable )
     - one stream is made of many different shards, and you gonna be paid per shard provisioned
 - Kinesis Firehose 
 - Kinesis Analytics: perform real time analytics on streams using SQL 
