# Amazon S3

## Bucket

This allows for object (files) storage in a "bucket" (directory).
There are some facts related to it:
- Buckets must have a globally unique name (across all regions and all accounts)
- Buckets are defined at the region level
- Buckets are created in a region (not global)
- Naming Convention (no uppercase, no underscore, 3-63 characters long,  must start with lowercase, must not start with prefix "xn-" or suffix "-s3alias")

## Objects

Are files which have a **key**, which represents the **full path** (e.g.: "s3://my-bucket/my_folder1/another_folder/my_file.txt"), and is compose of a **prefix** (e.g.: "my_folder1/another_folder") and the **object name** (e.g.: "my_file.txt").
**Note that are no concept of "directories" withing buckets.**

Some restrictions are in place for objects:
- Max object size is 5TB (5000GB)
- If uploading more than > 5GB => you must use "multi-part upload"

Objects are comprised of: 
- Value which is the content of the body
- Metadata (list of text key / value pairs - system or user metadata)
- Tags (Unicode key / value pair up to 10)
- Version ID (if versioning enabled)

### Versioning
You can version if it enabled at bucket level which will result in the same key overwrite to just change the version (any versioned prior to enable versioning will have version "null")

## Security

1. User-Based
  - IAM Policies - which API calls should be allowed for a specific user from IAM
2. Resource-Based
- Bucket Policies (bucket wide rules from the S3 console - allows cross account)
- Object Access Control List (ACL) - finer grain (can be disabled)
- Bucket Access Control List (ACL) - less common (can be disabled)

**Note**: an IAM principal can access an S3 object if:
- The user IAM permissions ALLOWS it or hte resource policy ALLOWS it
- **AND** there's no explicit DENY

### S3 Bucket Policies

JSON based policies
- Resources: bucket and objects
- Effect: Allow/Deny
- Actions: Set of API to Allow or Deny
- Principal: the account or user to apply the policy to

Use it for:
- Grant public access to the bucket
- Force objects to be encrypted at upload
- Grant access to another account (Cross Account)

## Replication (CRR & SRR)

It requires versioning to be enabled in source and destination buckets. It is of 2 types:
- Cross-Region Replication (CRR) (e.g.: used in compliance, lower latency access, replication accross accounts
- Same-Region Replication (SRR) (e.g.: log aggregation, live replication between production and test accounts)

Note that copying is asynchronous and you must give proper IAM permissions to S3.

## Storage Classes

**Durability** = how long objects stored are expected to incur losses on average (e.g.: High Durability: 99.999999999% => 10,000,000 boject you can expect 1 single object loss every 10,000 years). For S3 is the same for all storage classes.
**Availabity** = measures how readility available a service is, varies depending on storage class

S3 Standard - General Purpose
S3 Standard-Infrequent Access (IA)
S3 One Zone-Infrequent Access
S3 Glacier Instant Retrieval
S3 Glacier Flexible Retrieval
S3 Glacier Deep Archive
S3 Inteligent Tiering
