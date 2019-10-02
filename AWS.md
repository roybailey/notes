# AWS Notes

## Glossary

* **REGION** = area of world with 2+ Availability Zones
* **AVAILABILITY ZONE** = Data Centre (or Data Centres that are too close to be considered alternative failover DC)
* **EDGE LOCATION** = Caching content; Content Delivery Network (CDN) e.g. ***CloudFront***
* **VIRTUAL PRIVATE CLOUD (VPC)** = virtual network dedicated to a single AWS account. Logically isolated from other virtual networks in the AWS cloud, providing compute resources with security and robust networking functionality.

## AWS Account

Created account with `roybaileybiz@gmail.com` as `Professional` and with business debit card.

Activated MDA and captured qr-code if authentication device is lost

  

IAM to setup users, groups, roles. Assign Administrator/FullAccess for superuser

  

CloudWatch to setup billing alarms

  

S3 = Simple Storage Service

-   Object based, upload files up to 5tb
    
-   Unlimited storage, stored in buckets = folder in the cloud
    
-   S3 = universal namespace
    
-   Bucket = globally unique (gets url e.g. [https://s3-eu-west-1.amazonaws.com/](https://s3-eu-west-1.amazonaws.com/)<bucketname>)
    
-   Upload to S3 = 200 code = OK
    
-   Object
    

-   Key (filename)
    
-   Value (bytes of data)
    
-   VersionID (versioning)
    
-   Metadata (data about the data being stored e.g. belongs to)
    
-   Subresources e.g. Access Control List (permissions at bucket or object), Torrents
    

-   Consistency on S3
    

-   Read after write consistency for PUTs of new Objects
    
-   Eventual consistency for overwrite PUTs and DELETEs
    

-   Guarantees = 99.99% availability, 99.(11x9)% durability, designed to sustain loss of 2 facilities concurrently
    
-   Tiered storage
    
-   Lifecycle management (move to different tiers)
    
-   Versioning
    
-   Encryption at rest
    
-   MFA for deleting objects
    
-   Secure using Access Control lists and Bucket Policies
    
-   Storage Classes
    

-   S3 Standard
    
-   S3 - IA (Infrequently Accessed)
    
-   S3 One Zone - IA = lower cost, one availability zone
    
-   S3 - Intelligent Tiering = designed to optimize storage movements based on ML
    
-   S3 Glacier = low cost, retrievable in minutes/hours
    
-   S3 Glacier Deep Archive = lowest cost, 12 hours retrieve time
    

-   Billing
    

-   Storage use
    
-   Number of requests to objects
    
-   Storage management pricing (tiers)
    
-   Data Transfer Pricing
    
-   Transfer Acceleration = Caching in Edge Locations using CloudFront
    
-   Cross Region Replication Pricing = replication from one region to another
    

-   Make Public at Bucket or File level (File only if Bucket level is set)
    
-   By default all Buckets are Private
    

-   Bucket Policies = Bucket level access control
    
-   Access Control = File level access control
    

-   Create access logs
    

S3 - Encryption

  

-   Encryption In Transit
    

-   https is encrypted = SSL/TLS
    

-   Encryption at Rest
    

-   Encryption on storage device
    

-   Server Side Encryption
    

-   S3 Managed Keys (encrypt/decrypt keys) SSE-S3
    

-   SSE = Server Side Encryption
    

-   AWS Key Management Service, Managed Keys SSE-KMS
    
-   SSE with Customer Provided Keys SSE-C
    

-   Client Side Encryption
    

-   Encrypt at source and upload encrypted file data
    

  

S3 - Versioning

-   Versioning on bucket cannot be undone
    
-   Integrates with Lifecycle rules (to move things to Glacier storgate)
    
-   MFA Delete capability
    
-   Uploading new version of file overwrites public access by default
    
-   All versions taking up space, architectural consideration
    
-   Stores all versions
    
-   Deleting the file only adds a Delete Marker
    
-   Versions (inc. Delete Marker) can be deleted to restore older versions
    

  

S3 - LifeCycle Rules

-   Applied on Bucket under Management tab
    
-   Create a Rule, use Tags or file prefix pattern to match Files to apply Rule to
    
-   Setup transitions for current and previous versions to move to other tiers (S3-IA, Glacier)
    

  

S3 - Cross Region Replication

-   Applied on Bucket under Management tab - See Replication
    
-   Replication requires Versioning on both source and target buckets
    
-   Replicate Bucket, or Files based on Tags/Prefix
    
-   Replicate to lower cost storage
    
-   Destination will be another Bucket in different Region
    
-   Only replicates new uploads, not existing Files
    
-   Delete Markers are not replicated as part of CRR rule
    
-   Does not replicate Deletes on Versions themselves
    

  

S3 - Transfer Acceleration

-   CloudFront Edge Network used
    
-   Distinct URL to upload directly to edge location
    
-   Uploads then transferred via AWS fast network to your bucket
    

Cloud Front
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MDM2NDk2NTIsMTcwNTkxNDMyMywtND
g5MjUxNTA0LC0xNzczNDk0NDMyXX0=
-->