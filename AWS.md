# AWS Notes

## Glossary

* **REGION** = area of world with 2+ Availability Zones
* **AVAILABILITY ZONE** = Data Centre (or Data Centres that are too close to be considered alternative failover DC)
* **EDGE LOCATION** = Location Content is Cached; Different to Region/AZ (used by CDN)
* **VIRTUAL PRIVATE CLOUD (VPC)** = virtual network dedicated to a single AWS account. Logically isolated from other virtual networks in the AWS cloud, providing compute resources with security and robust networking functionality.
* **S3** = Simple Storage Solution
* **CDN** = Content Delivery Network
* **ORIGIN** = Origin of files distributed by CDN
* **DISTRIBUTION** = Collection of Edge Locations
* **EBS** = Elastic Block Store = Virtual Hard Disk in Cloud
* **AMI** = Amazon Machine Image

https://d0.awsstatic.com/whitepapers/aws_pricing_overview.pdf

## IAM

* A global service
* Users, Roles, Groups, Policies
* Policies attached to Groups, Users attached to Groups inherit group policies
* Root Account = account that first created the AWS account
* New Users have no permission when created
* New Users assigned Access Key ID and Secret Access Key (Viewable once)


## S3

* **Object based**, upload files up to 5tb
* Unlimited storage, stored in **Bucket** = folder in the cloud
* S3 is a **Universal Global Namespace**
* **Bucket = Globally Unique name**
  * Gets own url e.g. `https://s3-eu-west-1.amazonaws.com/<bucketname>`
* Uploads to S3 = 200 code = OK
* Supports
  * Tiered storage
  * Lifecycle management (move to different tiers)
  * Versioning
  * Encryption at rest
  * MFA for deleting objects
  * Secure using Access Control lists and Bucket Policies

### S3 Bucket Object (aka File)

  * Key (filename)
  * Value (bytes of data)
  * VersionID (versioning)
  * Metadata (data about the data being stored e.g. belongs to)
  * SubResources e.g. Access Control List (permissions at bucket or object), Torrents

### S3 Consistency

* Read after write consistency for PUTs of new Objects
* Eventual consistency for overwrite PUTs and DELETEs
* Guarantees
  * 99.99% availability
  * 99.99999999999% durability (called 11 9s)
  * Designed to sustain loss of 2 facilities concurrently

### S3 Storage Classes

* **S3 Standard**
* **S3 - IA** = Infrequently Accessed but fast access
* **S3 One Zone - IA** = lower cost, Infrequently Accessed in one availability zone only
* **S3 - Intelligent Tiering** = Designed to optimise storage movements based on Machine Learning (i.e. monitoring bucket/file real world usage to move and lower storage costs)
* **S3 Glacier** = low cost, retrievable in minutes or hours
* **S3 Glacier Deep Archive** = lowest cost, 12 hours retrieve time

### S3 Billing

* Storage use
* Number of requests to objects
* Storage management pricing (tiers)
* Data Transfer Pricing
* Transfer Acceleration = Caching in Edge Locations using CloudFront
* Cross Region Replication Pricing = replication from one region to another

### S3 Bucket/Object 

* `Make Public` at Bucket or File level (File only if Bucket level is set)
* By default all Buckets are Private
* Bucket Policies = Bucket level access control
* Access Control = File level access control
* Create access logs for auditing Bucket/Object access

### S3 - Encryption

* Encryption In Transit = **https** encryption = **SSL/TLS**
* Encryption at Rest = Encryption on storage device

#### Server Side Encryption (SSE)

* **SSE-S3** = S3 Managed Keys (encrypt/decrypt keys) 
* **SSE-KMS** = AWS Key Management Service, Managed Keys
* **SSE-C** = SSE with Customer Provided Keys

#### Client Side Encryption

* Encrypt at source and upload encrypted file data yourself

### S3 - Versioning

* Versioning on bucket cannot be undone
* Integrates with Lifecycle rules (to move things to Glacier storage)
* MFA Delete capability
* Uploading new version of file overwrites public access by default
* All versions taking up space, architectural consideration
* Stores all versions
* Deleting the file only adds a Delete Marker
* Versions (inc. Delete Marker) can be deleted to restore older versions

### S3 - LifeCycle Rules

* **Automates moving objects between storage tiers**
* Applied on Bucket under Management tab
* Create a Rule, use Tags or file prefix pattern to match Files to apply Rule to
* Setup transitions for current and previous versions to move to other tiers (S3-IA, Glacier)

### S3 - Cross Region Replication

* **Replicates to other regions**
* Applied on Bucket under Management tab - See Replication
* Replication requires Versioning on both source and target buckets
* Replicate Bucket, or Files based on Tags/Prefix
* Replicate to lower cost storage
* Destination will be another Bucket in different Region
* Only replicates new uploads, not existing Files
* Delete Markers are not replicated as part of CRR rule
* Does not replicate Deletes on Versions themselves

### S3 - Transfer Acceleration

* **Cache objects at Edge Locations**
* CloudFront Edge Network used
* Distinct URL to upload directly to edge location
* Uploads then transferred via AWS fast network to your bucket

### CloudFront

* **CDN** = Content Delivery Network
* **EDGE LOCATION** = Location Content is Cached; Different to Region/AZ
* **ORIGIN** = Origin of files distributed by CDN
* **DISTRIBUTION** = Collection of Edge Locations
* Can be used to delivery dynamic, static, streaming, interactive content
* Requests for content routed to nearest Edge Location for best performance
* Web Distribution = Websites
* RTMP = Streaming
* Objects cached for Time To Live (TTL) - configurable
* Caches can be cleared (charged for invalidating the cache)

#### CloudFront - Create Distribution

* Not available on free tier
* Takes ~1 hour to startup
* Must be Disabled before Deleting

### Snowball

* **Huge disk, get data into and out of AWS**
* Cheaper than high speed internet
* Tamper resistant enclosures
* 256-bit encryption
* Trusted Platform Module (TPM)
* Once transferred, verified and processed, Snowball appliance is wiped
* **Snowball Edge** = compute and storage (portable AWS, can be clustered)
* **SnowMobile** = shipping container

### Storage Gateway

* **Replicates your data from your own Infrastructure into AWS**
* VM image you can download and install on your infrastructure
* **File Gateway** = NFS & SMB
  * Connects your Application Servers to S3
  * Used for flat files
* **Volume Gateway** = iSCSI = Stored Volumes & Cached Volumes
  * Looks like a standard disk drive
  * Stored in cloud as EBS snapshots
  * *Stored locally, async backed up as EBS snapshots in AWS*
  * Cached volumes keep current data only in on-premise storage
* **Tape Gateway** archiving your data in the cloud
  * Uses tape based infrastructure

## EC2

* **Elastic Compute in the Cloud**
* Allows you to scale up and down as required
* Pricing
  * On Demand = Fixed rate by the hour or second
  * Reserved = 1 or 3 year contract term
    * Standard = 75% off
    * Convertible = 54% off but can have attributes changed (e.g. memory, cpu)
    * Scheduled = specific spiked traffic e.g. market opens needs more login capacity for 2 hours
  * Spot = Cheap spare capacity (bid at price you want)
* Dedicated Hosts = EC2 servers, helps with strict licensing like Oracle
  * Single tenant 
* EBS backed instance, default for root EBS volume to be deleted once instance is terminated
* EBS root volumes cannot be encrypted, only additional volumes can

### EC2 - Instance Setup

* Choose VM image
* Protect against Termination
* Add volumes (only extra volumes can be encrypted)
* Asymmetric encryption
  * Public Key = Padlock = Goes on EC2 Instance
  * Private Key = Padlock key itself = Locally on your machine, used to ssh into instance
  * Create public/private key e.g. `MyUSE1KP` for My US East 1st instance Key Pair
  * Download `MyUSE1KP.pem`
  * `chmod 400 MyUSE1KP.pem` to restrict to read only
* `ssh ec2-user@52.90.162.42 -i MyUSE1KP.pem` to login to running EC2 instance (check port 22 isn't blocked)
* `sudo su` to change from `ec2-user` to `root` user
* `yum update -y` to check and download updates 
* `yum install httpd` to install apache
* `service httpd start` to start apache (website home folder `/var/www/html`)
* `chkconfig on` to make sure apache is restarted if evert the EC2 server is restarted

### EC2 - Security Groups

* Security Groups are stateful e.g. Inbound security groups are automatically allowed Outbound
* All Inbound traffic blocked by default, Security Group enables specific protocol/port
* Changes to Security Group affective immediately
* Security Groups cannot block specific IP addresses, Network Access Control Lists do that instead

### EC2 - EBS

* **Elastic Block Store** = Virtual Hard Disk in Cloud
  * General Purpose SSD = most workloads = 16000 iops
  * Provisioned IOPS SSD = highest performance, databases = 64000 iops
  * Throughput Optimised HDD = low cost, big data, warehouses = 500 iops
  * Cold HDD = lowest cost, file servers = 250 iops
  * EBS Magnetic HDD = infrequently accessed data = 40-200
* Automatically replicated within Availability Zone

#### EBS Volume & Snapshots

* Wherever your **Volume is will be same Availability Zone as EC2 instance** (same zone for compute and disk)
* **Delete on Termination** on for root volume, not for added Volumes
* Ability to increase HDD sizes (might need to run cmd to tell OS new disk size)
* Ability to upgrade from General Purpose SSD to Provisioned IOPS SSD
* **Actions > Create Snapshot** to capture copy of disk
  * stored on S3
  * incremental snapshots are stored as deltas only to save space
  * snapshots of root volume should be taken when instance isn't running
* **Create Image** from snapshot instance using Hardware-assisted-virtualisation
* **AMIs** to launch new instance from your own images
* **Copy AMI** to new Region so we can launch in new Region
* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY4NzYyMjcxNCwxMTEyMjEyMjU5LDM5Mz
QzMTk3NCw2MTU0MDI5MzEsLTE0NzU4NDQyNTcsLTEwOTA5MTA4
NDAsMTAzNjc3NTI5NCwtNjU1MzM4MjMwLDE2NDQyMzg1NiwxMz
Y0NTM3OTE3LC0xNjk2NTQ4Nzc5LDM0NjQwMzM4MCw0NjcyMzcz
NjAsLTE1NDcwNzcwMDgsNTk5ODkxOTI4LDE3MDU5MTQzMjMsLT
Q4OTI1MTUwNCwtMTc3MzQ5NDQzMl19
-->