
1. A programmer is creating an application that requires signed requests (Signature Version 4) for invoking other AWS services. Having constructed a canonical request, created the string to sign, and calculated the signing information, which strategies can the programmer apply to finalize a signed request? (Select TWO.)
	1. When sending a request to AWS services using Signature Version 4, the signature should be included in the "Authorization" HTTP header or as a parameter "X-Amz-Signature" in the query string.
2. A Development team has deployed several applications running on an Auto Scaling fleet of Amazon EC2 instances. The Operations team have asked for a display that shows a key performance metric for each application on a single screen for monitoring purposes.
	1. Create a custom namespace with a unique metric name for each application
	2. Explanation: Therefore, the Developer should create a custom namespace with a unique metric name for each application. This namespace will then allow the metrics for each individual application to be shown in a single view through CloudWatch.
3. An application is using Amazon DynamoDB as its data store and needs to be able to read 100 items per second as strongly consistent reads. Each item is 5 KB in size. What value should be set for the table's provisioned throughput for reads?
	1. To determine the number of RCUs required to handle 100 strongly consistent reads per/second with an average item size of 5KB, perform the following steps:
	2. Determine the average item size by rounding up the next multiple of 4KB (5KB rounds up to 8KB).
	3. Determine the RCU per item by dividing the item size by 4KB (8KB/4KB = 2).
	4. Multiply the value from step 2 with the number of reads required per second (2x100 = 200).
4. An application uses both Amazon EC2 instances and on-premises servers. The on-premises servers are a critical component of the application, and a developer wants to collect metrics and logs from these servers. The developer would like to use Amazon CloudWatch. How can the developer accomplish this?
	1. Install the CloudWatch agent on the on-premises servers and specify IAM credentials with permissions to CloudWatch.
5. A Developer is designing a fault-tolerant environment where client sessions will be saved. How can the Developer ensure that no sessions are lost if an Amazon EC2 instance fails?
	1. Use Amazon DynamoDB to perform scalable session handling
	2. The **DynamoDB Session Handler** is a custom session handler for PHP that allows developers to use Amazon DynamoDB as a session store. Using DynamoDB for session storage alleviates issues that occur with session handling in a distributed web application by moving sessions off of the local file system and into a shared location. DynamoDB is fast, scalable, easy to setup, and handles replication of your data automatically.
6. An application uses Amazon API Gateway, an AWS Lambda function and a DynamoDB table. The developer requires that another Lambda function is triggered when an item lifecycle activity occurs in the DynamoDB table.
	1. Enable a DynamoDB stream and trigger the Lambda function synchronously from the stream
7. A company uses an Amazon S3 bucket to store a large number of sensitive files relating to eCommerce transactions. The company has a policy that states that all data written to the S3 bucket must be encrypted.
	1. Create an S3 bucket policy that denies any S3 Put request that does not include the x-amz-server-side-encryption
8. An application is being migrated into the cloud. The application is stateless and will run on a fleet of Amazon EC2 instances. The application should scale elastically. How can a Developer ensure that the number of instances available is sufficient for current demand?
	1. Create a launch configuration and use Amazon EC2 Auto Scaling
9. A Developer is designing a cloud native application. The application will use several AWS Lambda functions that will process items that the functions read from an event source. Which AWS services are supported for Lambda event source mappings? (Select THREE.)
	1. Amazon Simple Queue Service (SQS)
	2. Amazon DynamoDB
	3. Amazon Kinesis
10. An engineer is constructing an AWS Lambda function and intends to log specific key events that transpire during the function's execution. To correlate the events with a particular function invocation, the engineer is looking to incorporate a unique identifier. The following code segment has been added to the Lambda function: function handler (event, context) {}
	1. Use **context.awsRequestId** within the function to fetch the unique identifier associated with each invocation.