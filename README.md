# aws_solutions_architect_notes
## S3: simple storage service
- S3 is object-based ( it allows uploading files) 
- Files can be from 0-5TB 
- There is unlimited storage 
- Files are stored in buckets 
- S3 is a universal namespace: the name must be unique globally 
### S3-objects
- They consist of:
    - Key (the name of the data) 
    - Value (the data) 
    - VersionID (for versioning) 
    - MetaData (data about the data we are storing)
    - Subresources (access control list, torrent)
### Data consistency for s3
    - Read after write consistency (after uploading a file we can immediately read it)
    - Eventual consistency for overwrite for puts and deletes
### s3 features
    - Tiered storage 
    - lifeCycle management:Lifecycle policies allow you to automatically review objects within your S3 Buckets and have them moved to Glacier or have the objects deleted from S3. You may want to do this for security, legislative compliance, internal policy compliance, or general housekeeping.
    - Encryption 
    - Versionning: when deleting an object from a bucket where we applied versioning, it won’t be deleted automatically however a deletion mark will be putted on that object.

- AWS Multi-Factor Authentication (MFA) is a simple best practice that adds an extra layer of protection on top of your user name and password.
- When deleting an object from a bucket where we applied versioning, it won’t be deleted automatically however a deletion mark will be putted on that object  
### S3 classes
#### 1.  S3 Standard 
- It is really highly available and highly durable, stored across many devices in many facilities. 
#### 2.  S3 IA (Infrequently accessed) 
- This is basically for data that has access less frequently but requires rapid access when you need it.
#### 3.  S3 One zone accessed
- this is when you want a really low cost option for your infrequently accessed data and you don't even need it. You don't have to worry about multiple Availability Zones so it is literally just stored in one availability zone and it's infrequently access but you still need to be able to access that data instantly 
#### 4. S3 intelligent tiering 
- this is using machine learning and basically what it does is it looks at how often you use your objects and then it will move your objects around the different storage classes based on what it's learnt so we'll move it from S3 standard to S3 infrequently accessed because it knows that you don't access those files so that's S3 intelligent tearing.
#### 5. S3 Glacier 
- you can restore any amount of data and it's really super super cheap and then you can retrieve times configurable from minutes to hours.
#### 6. S3 Glacier deep archive 
- This is the lowest storage class the lowest cost storage class that you can buy. But your retrieval time is going to be 12 hours.


## Databases
- RDS: relational databases services is for online transaction processing (SQL, MySQL, PostgreSQL, Aurora, MariaDB, Oracle)
- RDS features key: 
    - Multi AZ: for disaster recovery when we lose the database in the first AZ ( the primary database ), amazon will update the DNS with the new IP address of the database in the second AZ. 
    - Read replicas:for performance that means the database is automatically updated ( after modifying the primary database ) and also it is used when we have so many users accessing the database we devise them into parts ; some access the primary, the other part access the secondary and so on.
- RDS runs on virtual machines
- you can’t access these O.S
- RDS is not serverless but aurora is serverless 
- Elasticache used to speed up the performance of existing databases
### Buckups types
- Automated backups: allow you to recover your database to any point in time within a retention period and the retention period can be between one and 35 days.
- Database snapshots:
    - Are done manually.
    -  
## EBS:
- Instance storage attached, the data will be lost if the instance is terminated.
- We can dettach an EBS from one instance and attach it to another.
- We can backup data on the EBS volumes to S3 by taking point-in-time snapshots.
- We can use Amazon Data LifeCycle  Manager to automate the creation, the retention, and the deletion of snapshots taken to back up the EBS.
- We can share unencrypted/encrypted (only if they are encrypted with a customer managed key) EBS snapshots.
- We can't share encrypted snapshots publicly. 
- We can't share encrypted EBS volumes.
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
  - Data at rest
  - All the data moving between the volume and the instance 
  - All snapshots created from the volume
  - All volumes created from these snapshots 
- ### changing the data's encrytion state: (encrypt unencrypted volume)
  - We can't remove encryption from an encrypted volume 
  - We can't directly encrypt an uncrypted volume, instead we can create an instance with an encrypted root device volume from an encrpted snapshot
  
## Instance store
- We can specify an instance store for an EC2 instance only when launching it
- Data in the instance store persists only when we reboot the EC2 instance, they will be lost if:
    - The instance is stoped or terminated
    - The hard disk driver fails
    
## Route 53: is the DNS service for Amazon
- DNS is on the port 53 that's why we got the name of route 53
- DNS is used to convert human friendly names into an internet protocol (IP) addresses 
- IP addresses:
    - IPV4: has a space of 32 bits => we have 4 billion different addresses
    - IPV6: has a space of 128 bits => we have 340 undecillion address 
- The start of authority record stores information about:
    - The name of the server that supplied the data for the zone.
    - The administrator for the zone 
    - The current version of the data file
    - The default nb of seconds for time to live (TTL) file on ressource records.
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
    - Given the choice, choose an alias record over a CNAME
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
- You can have only one record with multiple IP addr. If you specifyu multiple values in a record, route 53 returns all values to the user in a random order.
### 2. weighted routig
- Allows you to split your traffic based on different weights assigned (different regions) 
- Ex: you can set 10% of your traffic to go to us-east-1 and 20% to go to eu-west-1
### 3. latency-based routing 
 - Allows you to route your traffic based on the lowest network latency for your end user (ie which region will give you the fasteest response time)
### 4. failover routing 
 - Are used when you want to create an active/passive set up.
 - Ex: you may want your primary site to be in eu-west-2 and your secondary DR site to be in AP-southeast-2.
 - Route 53 will monitor the health of your primary site using a health check ( the health check monitors the health of your endpoints)
### 5. geolocation routing 
- Lets you choose where your traffic will be sent based on the geographic location of your users(ie the location from which DNS queries originate)
### 6. geoproximity routing 
- Lets amazon route 53 route traffic to your ressources based on the geographic location of your users and your ressources.
- You cn optionally choose to route more or less traffic to a given ressource by specifying z value known as bias.
- To use geoproximity routing, you must use route53 traffic flow.
### 7. multivalue routing 
- Lets you configure amazon route53 to return multiple values such as ip adrs for your web server, in response to a dns query.
- You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each ressource.
- It is similar to simple routing but it allows oyu to put health checks on each record set.

## VPC (virtual private cloud)
- We can have 5 VPCs per region
### Bastion host
is a server whose purpose is to provide access to a private network from an external network such as Internet

### Nat gateway 
- When we have NAT gateway attached to the subnet where web server ec2 resides, the traffic from the internet cannot reach the ec2.
- Nat gatewa only routes traffic from aws ressources within a vpc to the internet.
- Any traffic from the internet into vpc ressouces is not allowed.
- NAT Gateways don't have security groups
- We can't share NAT gateways across VPCs
- Provides better availability, higher bandwidth, and requires less administrative effort than NAT instances

### Security group  
- Are stateful 
- Can be understood as a firewal to protect EC2 instances
- In the default security group, all inboud and outbound traffic are allowed by default.
- We can create a custom security group, and by default it allows no inbound traffic and allows all outbound traffic.
- You cannot block specific IP addresses using Security Groups, instead use Network Access Control Lists.
- You can specify allow rules, but not deny rules.

### Network ACL  
- Are stateless: any change applied to the incoming rule isn't automatically applied to the outgoing rule.
- Can be understood as a firewall for the whole subnet.
- Rules are organised with chronological order; the rule n° 100 will trump the rule n°400
=> If we have a deny rule, it must be before the allow rule
- NACL will be evaluated before the security group.
- When creating  VPC, it will be associated with NACL and if we we create subnets, they will be associated by default to the default NACL of the VPC.
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
- It a web service that guves you access to message queue which is used to store messages while waiting for a computer to process them.
- Using SQS, you can decouple the components of an app, so they run independtly.
- Messages can contain up to 256KB of text in any format. any component can later retreive the messages programmatically using the amazon SQS API.
- A queue is a temporary repository for meassages that are waiting processing
### Queue types
    - Standard queues (default)
        - Unlimited throughput: it supports a nearly unlimited nb of transactions per sec.
        - Best-effort ordering: occasionally, msgs might be delevired in an order different from which they were sent.
        - At least one delivery: a msg is delivered at least one, but occasionally more than one copy of a msg is delivered.
    - FIFO queues 
        - High throughput: it supports up to 300 msgs per sec(send, receiveor delete operations per second)
        - First In First Out delivery: this order is strictly preserved.
        - Exactly-one processing: a msg is delivered once and remains available until a consumer process and deletes it. Duplicates are not intoduced in the queue.
### Dead letter Queue
- The main task of a dead letter is handling message failure.
- It lets you set aside and isolate a msg that can't be processed correctly to determine why their processing didn't succeed.
### Delay Queue
- It is the period of time during it the msg remains invisible and can't be executed.
- It can be from 0 up to 12hours, the default timeout is set to 30 seconds.
- After the delay period has been expired, the msg becomes visible & can be executed.
- Immediately after a msg is received, it remains in the queue. to prevent other consumers from processing the msg again, Amazon SQS sets a visibility timeout to prevent other consumers from receiving & processing the msg.
### SQS long polling vs SQS short polling 
- SQS long polling is a way to retrieve messagages from SQS queues- waits for the messages to arrive.
- SQS short polling returns immediately ( even if the message queue is empty) 
- SQS long polling can reduce costs.
- SQS long polling can be enabled at the queue level or at the API level using WaitTimeSeconds. 
### SQS remarks
- For any question about decoupling the infrastructure, think about SQS

## SNS (Simple Notification Service)
- It is a web service that makes it easy to set up, operate, and send notifications from the cloud.
- It is a publish/subscriber syst
- It provides developers with a highly scalable, flexible, and cost-effective capacity to publish messages from an app and immediately deliver them to subscribers or other apps.
- It allows you to group multiple recipients using topics.
- To prevent messages from being lost, all messages published to Amazon SNS are stored redundantly across multiple AZ.
- Each meassage attribute consists of: a name, a type and a value.
### SNS Benefits 
- Push based delivery 
- Simple & easy to integrate 
- Pay as you go model with no up front costs.
## SWF (Simple Workflow Service)
- it has a way of combining the digital environment with manual tasks ( with human being ) .remember the example of the book order
## API gateway 
- It is a fully managed service that makes it easy for developers to publish, monitor and secure APIs at any scale.
- It is just like a front door to the AWS environment.
- API caching: (Caching is the ability to store copies of frequently accessed data in several places along the request-response path)
- You can enable API caching in Amazon API Gateway to cache your endpoint's responses. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of requests to your API.
- 
## Kinesis 
- It is a managed alternative to Apache Kafka (is an open-source distributed event streaming platform used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications)
- great for application logs, metrics, IoT, clickstreams
- Great for real-time big data
- Great for streaming processing frameworks.
- Data is automatically replicated synchronously to 3 AZ.
### Kinesis types
- Kinesis Streams: low latency streaming ingest 
    - Streams are devided into ordered shards (partitions)
    - Data retention is 24h by default, can go up to 7 days 
    - Ability to reprocess/replay data: once you consume the data, the data is not gone from kinesis, it will be gone from the data retention period but you can reuse it               over and over
     - Multiple applications can consume the same stream => that means it provides real time processing with scale of throughput
     - Once you isert data into kinesis, it can't be deleted ( it is immutable )
     - The data capacity of a stream is a function of the nb of shards that you have specified for the stream.
     - The total capacity of the stream is the sum of the capacities of its shards.
     - Shard limits: 
        - for a producer: 1MB/s or 1000 msgs/s at write per shard, if you go over that limit you'll get ProvisionedThroughputException
        - for a consumer (classic): 2MB/s at read per shard across all consumers & 5 API calls per second per shard across all consumers 
 - Kinesis Firehose 
    - Near real time: It loads new data into your destinations within 60 seconds after the data is sent to the service. As a result, you can access new data sooner and react to business and operational events faster.
    - the data are not persistent, there is no retention period
 - Kinesis Analytics:
    - perform real time analytics on streams using SQL 
## Container Orchestration tools
- it is a tool for managing, scaling and deploying containers. e.g kubernetes, docker swarm, ECS, etc
### ECS (Elastic Container Service)
- It is a container orchestration service
- It manages the whole container lifeCycle (start, re-schedule, load balance)
#### How does ECS work ?
    - Run containerized application cluster on AWS: 
        - ECS cluster is just like the control plane which contains all the EC2 instances that are running containers & it can manage each container lifeCycle.
        - The EC2 instances are hosting the containers.
        - On the EC2 instances, we will have the container runtime so that containers can run.
    - We will have also an ECS agent, for the controll plane to communicate with each individual EC2 instance and to manage them.
    - you have to manage the EC2 instances ( compute fleet ) & join them to the ECS cluster.
    - When launching a new container, make sure you have enough ressources for it.
    - you have to manage the O.S, the Runtime and the ECS agent on the EC2 instances
#### AWS Fargates:
- we can use AWS Fargate, when we want to delegate infrastructure management to AWS.
- It is an alternative to EC2 instance
- It is a serverless way to launch containers
- Pay only for what you use
- Fargate vs EC2 pricing:
    - for forgate, you pay for how long the container is running & for how much capacity was used to run it
    - for EC2, you pay for the whole server
- Remark: if you need to access to the infrastructure, EC2 will be a better choice.
#### ECS components 
- cluster: logical collection of ECS ressources-either EC2 instances or fargate instances.
- Task definition: JSON template that describes containers which forms your application.can contain multiple containers up to a maximum of 10.
- Container Definition: inside a task definition, it defines the individaul containers a task uses. it controlls CPU, memory allocation & port mappings.
- Task: single running copy of any containers defined by a task definition. one working copy of an app e.gh DB & web applications.
- Service: allows task definitions to be scaled by adding tasks. It defines minimum & maximum values.
- Registry: storage for container images (e.g ECR or docker hub). used to donload images to create containers.
### EkS (Elastic kubernetes Service)
- It is an elastic kubernetes service. for managing kubernetes cluster on AWS infrastructure.
- It is an alternative to ECS.
#### ECS vs EKS 
- EKS:
    - you already use K8s
    - Easier to migrate to another plateform (GCM, azure, etc), however the migration can be difficult when using a lot of other aws services. it may that services are not on other plateform
    - it has a large commmunity (a lot of documentation due to the fact that k8s is an open source)
- ECS:
     - specific to AWS.
     - migration is difficult 
     - less complex applications
     - ECS control plane is free
#### How does EKS work ?
1. create a cluster(represents the control plane): it is the master node
    - the master node is replicated on 3 AZ.
2. create worker nodes
    - create EC2 instances & connect them to the EKS cluster
- the EKS cluster will communicate with the worker nodes via k8s worker processes
- remark: when creating the woreker nodes:
    - EKS with EC2 instances : self-managed
        - you need to manage the infrastructure for the worker nodes
    - EKS with Node group: semi-managed
        - you can group your worker nodes in a node group & the node group can handle some of the heavy lifting for you.
    - EKS with Fargate: full-managed
## ECR (Elastic Container Registry)
- it is a repository for docker images
- A private docker repository 
- Stores, manages & deploys docker container images 
- advantages of using ECR:
    - integrates with other AWS services, it notifies when a new version of an image gets pushed into the registrey and then dowloads it into your cluster.
   
## Load balancer 
- It is a physical or virtual device that's designed to help you to balance the network load.
- we have 3 types of load balancing:
    1. Classic load balancer (v1-old generation)
    2. Application load balancer (v2-new generation)
    3. Network load balancer (v2-new generation) 
- It is recommended to use the newer generation as they provide more features.
### Health checks 
  - Are crucial for load balancers to know if the instances they are forwarding traffic to are availables to reply to requests or not.
### ALB (Application Load balancer)
- Called layer 7
- Load balance to multiple HTTP apps across machines
- Load balance to multiple apps on the same machine
- Load balance based on route in URL or hostname in URL.
- Basically, are awsome for microservices & container-based app (e.g docker & amazon ECS)
- In comparison, we would need to create one classic load balancer per application before, it was very expensive & inefficient.
- support HTTP/HTTPS and sockets protocols
- The application servers don't see the IP of the client directly 
    - The true IP of the client is inserted in the header X-Forwarded-For
    - we can also get port (X-Forwarded-Port) & proto (X-Forwarded-Proto)
### NLB (Network Load balancer)
- Forward TCP traffic to the instances.
- Handle millions of requests per second.
- Support both static & elastic IP.
- Less latency 100ms (vs 400ms for ALB).
- Are mostly used for extreme performance & should not be the default load balancer you choose.
### Good to know 
- LBs can scale but not instantaneously, contact AWS for a warm-up.
- NLB directly see the client IP
- 4xx errors are client induced errors
- 5xx errors are app induced errors.
## CloudFormation
- Is a declarative way of outlining the AWS infrastructure, for any resources (most of them are supported)
## AWS Global Accelerator
 - Is a networking service that improves the performance of your users’ traffic by up to 60% using Amazon Web Services’ global network infrastructure. When the internet is congested, AWS Global Accelerator optimizes the path to your application to keep packet loss, jitter, and latency consistently low
