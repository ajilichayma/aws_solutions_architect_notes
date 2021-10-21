# aws_solutions_architect_notes
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
  
## Instance store: 
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
- Records types:
    - A record: stands for address record, it maps a name to one or more IP addresses.
    - CNAME record: a canonical name, can be used to resolve one domain name to another.
    - Alias record: can be used to resolve one domain name to another
    - The CNAME can't be used for naked domain names (naked domain= base= bare= root apex), howver the alias record can.
    - The A record must resolve to an IP. The CNAME and ALIAS records must point to a name.
- aws routing policies: 
    1. simple routing
    2. weighted routig
    3. latency-based routing 
    4. failover routing 
    5. geolocation routing 
    6. geoproximity routing 
    7. multivalue routing 
