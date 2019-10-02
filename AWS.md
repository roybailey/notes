# AWS Notes

## Glossary

* **REGION** = area of world with 2+ Availability Zones
* **AVAILABILITY ZONE** = Data Centre (or Data Centres that are too close to be considered alternative failover DC)
* **EDGE LOCATION** = Caching content; Content Delivery Network (CDN) e.g. ***CloudFront***
* **VIRTUAL PRIVATE CLOUD (VPC)** = virtual network dedicated to a single AWS account. Logically isolated from other virtual networks in the AWS cloud, providing compute resources with security and robust networking functionality.
* **S3** = Simple Storage Solution

## AWS Account

* Created account with `roybaileybiz@gmail.com` as `Professional` and with business debit card.
* Activated MDA and captured QR-Code (used if authentication device is lost)
* Downloaded Google Authenticator App
* Created `roy.bailey` IAM users in `Developers` group and assigned `Administrator/FullAccess` for users in this group
* Used **CloudWatch** to setup a billing alarm, for when costs > $20 p/month

## S3

* Object based, upload files up to 5tb
* Unlimited storage, stored in buckets = folder in the cloud
* S3 is a universal global namespace
* Bucket = Globally Unique name
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

* `S3 Standard`
* `S3 - IA` = Infrequently Accessed but fast access
* `S3 One Zone - IA` = lower cost, Infrequently Accessed in one availability zone only
* `S3 - Intelligent Tiering` = Designed to optimise storage movements based on Machine Learning (i.e. monitoring bucket/file real world usage to move and lower storage costs)
* `S3 Glacier` = low cost, retrievable in minutes or hours
* `S3 Glacier Deep Archive` = lowest cost, 12 hours retrieve time

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

* Encryption In Transit = `https` encryption = `SSL/TLS`
* Encryption at Rest = Encryption on storage device

#### Server Side Encryption (SSE)

* `SSE-S3` = S3 Managed Keys (encrypt/decrypt keys) 
* `SSE-KMS` = AWS Key Management Service, Managed Keys
* `SSE-C` = SSE with Customer Provided Keys

#### Client Side Encryption

* Encrypt at source and upload encrypted file data yourself

### S3 - Versioning

* Versioning on bucket cannot be undone
* Integrates with Lifecycle rules (to move things to Glacier storgate)
* MFA Delete capability
* Uploading new version of file overwrites public access by default
* All versions taking up space, architectural consideration
* Stores all versions
* Deleting the file only adds a Delete Marker
* Versions (inc. Delete Marker) can be deleted to restore older versions

### S3 - LifeCycle Rules

* Applied on Bucket under Management tab
* Create a Rule, use Tags or file prefix pattern to match Files to apply Rule to
* Setup transitions for current and previous versions to move to other tiers (S3-IA, Glacier)

### S3 - Cross Region Replication

* Applied on Bucket under Management tab - See Replication
* Replication requires Versioning on both source and target buckets
* Replicate Bucket, or Files based on Tags/Prefix
* Replicate to lower cost storage
* Destination will be another Bucket in different Region
* Only replicates new uploads, not existing Files
* Delete Markers are not replicated as part of CRR rule
* Does not replicate Deletes on Versions themselves

### S3 - Transfer Acceleration

* CloudFront Edge Network used
* Distinct URL to upload directly to edge location
* Uploads then transferred via AWS fast network to your bucket

### Cloud Front


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NDcwNzcwMDgsNTk5ODkxOTI4LDE3MD
U5MTQzMjMsLTQ4OTI1MTUwNCwtMTc3MzQ5NDQzMl19
-->