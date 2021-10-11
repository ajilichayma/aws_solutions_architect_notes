# aws_solutions_architect_notes
## EBS:
- instance storage attached, the data will be lost if the instance is terminated.
- we can dettach an EBS from one instance and attach it to another.
- we can backup data on the EBS volumes to S3 by taking point-in-time snapshots
- we can use Amazon Data LifeCycle  Manager to automate the creation, the retention, and the deletion of snapshots taken to back up the EBS.
- we can share encrypted/unencrypted EBS snapshots.
- ### EBS-optimized :
  uses an optimized configuration stack and provides additional, dedicated capacity for amazon EBS I/O. this optimization provides a best performance for EBS by 
  minimizing contention between EBS I/O and other traffic.
- ### EBS encrytion: 
  when creating an encrypted EBS volume, the following types of date will be encrypted
  - data at rest
  - all the data moving between the volume and the instance 
  - all snapshots created from the volume
  - all volumes created from these snapshots 
- ### changing the data's encrytion state: (encrypt/unencrypted unencrypted/encrypt volume)
  - 
## Instance store: 
