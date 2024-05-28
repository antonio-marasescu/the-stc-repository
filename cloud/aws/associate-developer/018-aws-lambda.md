
# AWS Lambda

AWS Lambda is a compute service where you can upload your code and create a Lambda function. AWS Lambda takes care of provisioning and managing the servers that you use to run the code. You don't have to worry about OSs, patching, scaling, etc. You can use Lambda in the following ways:
- As an event-driven compute service, where AWS Lambda runs your code in response to events. These events could be changes to data in an AWS S3 bucket or an AWS DynamoDB table.
- As a compute service to run your code in response to HTTP requests using AWS API Gateway or API calls made using AWS SDKs.
## Benefits

- Avoid limitations of EC2 Servers:
    - Virtual Servers in the Cloud
    - Limited by RAM and CPU
    - Continuously running
    - Scaling means intervention to add / remove servers
- Lambda provides:
    - Virtual functions
    - no servers to manage
    - Quick, short executions
    - Run on-demand
    - Scaling is fully automated
- Cost Optimization:
	- Easy Pricing:
	- Pay per request and compute time
	- Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
	-
- Integrations and Monitoring:
	- Integrations with the entire AWS Stack
	- Integrated with many programming languages
	- Easy monitoring through AWS CloudWatch
	- Easy to get more resources per functions (up to 3GB of RAM)
	- Increasing RAM will also improve CPU and network
### Language Support

- Node.js (JavaScript)
- Python
- Java (Java 8 compatible) • C# (.NET Core)
- Ruby
- Golang
- C#
- PowerShell
## Pricing

Pricing is calculated the following way:
- Number of requests
    - First 1 million requests are free. $0.20 per 1 million requests thereafter.
- Duration
    - Duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB-second used.

## Configuration

- Timeout: default 3 seconds, max of 15 minutes
- Environment variables
- Allocated memory (128M to 3G)
- Ability to deploy within a VPC + assign security groups
- IAM execution role must be attached to the Lambda function
## Invocations

### AWS Lambda Synchronous Invocation

- Result of the function is returned right away
- Synchronous invocation happens when the function is invoked as follows:
	- User invoked:
	    - ELB (Application Load Balancer)
	    - API Gateway
	    - CloudFront
	    - S3 Batch
	    - CLI
	    - SDK
	- Service invoked:
	    - Cognito
	    - Step Functions
	    - DynamoDB Streams
	- Invoked from other services:
	    - Amazon Lex
	    - Alexa
	    - Kinesis Data Firehose
- Error handling must be done on the client side
- Invoke function from a command line:

```
aws lambda invoke --function-name hello-world --cli-binary-format raw-in-base64-out --payload '{}' --region eu-west-2 response.json 
```

#### Lambda integration with ELB (ALB)

- Used for exposing a HTTP(S) endpoint
- The function must be registered in a target group
- HTTP request is converted by the ALB to JSON and the JSON is passed to the function an input
- The ALB converts the lambda response back to JSON
- Multi-header values: by default they are disabled. When enabled HTTP headers and query string parameters that are sent with multiple values are shown as arrays within the Lambda event and response object

#### Lambda@Edge

- Lambda function deployed synchronously from CDN using CloudFront
- Used for running global AWS Lambda function alongside CDN
- Can be used for request filtering before reaching the final application
- Can change CloudFront requests and responses:
	- After CloudFront receives a request from viewer (view request)
	- Before CloudFront forwards a request to origin (origin request)
	- After CloudFront receives the response from the origin (origin response)
	- Before CloudFront forwards the response to the viewer (viewer response)
- Use cases:
	- Website security and privacy
	- Dynamic web application at the Edge
	- Search engine optimization (SEO)
	- Intelligent route across origins and data centers
	- Bot mitigation at the Edge
	- Real-time image transformation
	- A/B testing
	- User authentication/authorization
	- User prioritization
	- User tracking and analytics

### AWS Lambda Asynchronous Invocation

- Mainly invoked after a certain event
- The events are placed in an internal event queue
- Automatic retry: 3 retries: first one right after failure, second one waits 1 minute, third one after 2 minutes (exponential backoff)
- Lambda function should be idempotent - in case of retries the result should be the same
- Retries can cause duplicate log entries in CloudWatch logs
- DQL: in case of failure after retries the lambda function can send an event to SNS or SQS
- Services that invoke async lambda functions:
	- S3
	- SNS
	- CloudWatch Events / EventBridge
	- CodeCommit
	- CodePipeline
	- CloudWatch Logs
	- AWS SES
	- CloudFormation
	- Amazon Config
	- Amazon IoT / IoT Events
- Invoke a function asynchronously from CLI:
```
aws lambda invoke --function-name hello-world --cli-binary-format raw-in-base64-out --payload '{}' --region eu-west-2 --invocation-type Event response.json 
```

- Invocation from CloudWatch Events / EventBridge possible origins:
	- Serverless CRON job
	- CodePipeline state change
- Invocation from S3 events:
	- Events: _S3:ObjectCreated_, _S3:ObjectRemoved_, etc.
	- Possible to do object filtering by name (*.jpg)
	- Possible use case: generate thumbnails for newly uploaded images
	- S3 event notification typically deliver events in seconds, sometimes it can take longer
	- If two writes are happening on a single, non-versioned object at the same time, it is possible that only one event gets delivered (solution: enable versioning on the S3 bucket)

## Event Source Mapping

This happens when records needs to be polled from the source (e.g.: SQS queues, Kinesis Data Streams, DynamoDB Streams)

- Lambda is invoked synchronously
- There are 2 categories of event source mapper:
	- Stream (Kinesis Data Streams, DynamoDB)
		- Event source creates an iterator for each shared so data can be processed in order
		- New items, from beginning or timestamp
		- Processed items ae not removed from the stream
		- Low traffic: use batch window
		- High traffic: multiple batches can be processed in parallel. Up to 10 batch processes per shard, in-order processing for each partition key
		- Errors: the entire batch is reprocessed in case of an error until the function succeeds or the items in the batch expire
		- Configure event source mapping to handle errors:
			- discard old events
	        - restrict number of retries
	        - split the batch on error (mainly used in case of timeout issues)
	        - discarded events can go to a *Destination*
	- Queues (SQS):
		- Event source mapping uses long polling to poll the queue
		- Batch size: 1-10 messages
		- Recommendation: set the queue visibility to _6x the timeout of the Lambda function_
		- DLQ: set-up the DLQ on the queue, not on the Lambda
		- Lambda supports FIFO queues, scaling up to the number of active message groups
		- Lambda scales up to process a standard queue as fast as possible
		- In case of error batches are returned as individual items => they might be process as part of different batch
		- Idempotent processing in case of duplicates
		- Lambda deletes the processed items from the queue
- Scaling:
    - Data streams:
        - One Lambda invocation per each shard
        - Parallelization: up to 10 batches per shard in the same time
    - Queues:
        - Lambda adds 60 more instances per minute to scale up
        - Up to 1000 batches of messages simultaneously
    - FIFO queues:
        - Messages within the same group are processed in order
        - Lambda scales to the number of the message groups
        
## Destinations

- Configure to send result of a function to a destination
- Async invocations destinations for successful and failed events can be sent to:
	- SQS
	- SNS
	- Other Lambda
	- EventBridge bus
- Use destination instead of DLQ (even if both can be used at the same time)
- In case of event source mapping: for discarded batches, event batches can be sent to SQS and SNS

## IAM Roles for Lambda

- Grants permission to Lambda to access other AWS services
- When we use an event source mapping to invoke a function, Lambda uses the execution role to read event data.
- Best practice: one execution role per function
- Lambda resource based policy: give other accounts of AWS services to use Lambda **resources**
- IAM principal can access Lambda:
	- if the IAM policy attached to the principal authorizes it
	- if the resource based policy authorizes it

## Environment Variables

- Env. variable = key / value pair
- Adjust the system behavior
- Env. variables are available to the function code
- Lambda has its own env. variables
- Env. variables can be encrypted (KMS), useful for secrets

## Logging / Monitoring

- Integration with CloudWatch logs if the lambda has the IAM policy
- CloudWatch Metrics:
    - Information about invocation, duration, concurrent execution
    - Error count, success rate, throttles
    - Failures
    - Iterators for streams
- Tracing with X-Ray using X-Ray SDK in code
- Env. variables for X-Ray:
    - **_X_AMZN_TRACE_ID**
    - **AWS_XRAY_CONTEXT_MISSING**
    - **AWS_XRAY_DAEMON_ADDRESS** - X-Ray Daemon _IP_ADDRESS:PORT_

## Networking

- By default: Lambda is launched outside of user VPC, so it can not access resources from inside of a private VPC
- Lambda can be deployed in a VPC:
    - Lambda will create an ENI (Elastic Network Interface)
    - Needed _AWSLambdaVPCAccessExecutionRole_
- By default: Lambda in a user defined VPC can not access internet (even if the VPC is public). Solution: NAT Gateway / Instance, Lambda deployed in a private subnet
- DynamoDB can be accessed via VPC endpoints (or via public internet)
- CloudWatch logs work in private subnet without NAT

## Configuration / Performance

- RAM:
    - Can be scaled from 128MB to 3008MB in 64MB increments
    - The more RAM we add, the more vCPU credit we get (vCPU can not be configured separately)
    - At the moment 1792MB a function has the equivalent of a full vCPU
    - After this value we get more than one vCPU, multithreading code can leverage this
- Timeout: by default 3 seconds, max 900 seconds (15 minutes)
- Execution context:
	- Temporary runtime env. that initializes any external dependency
	- Great for DB connections, HTTP clients
	- Is maintained for some time in anticipation of another Lambda invocation
	- Does include the **/tmp** directory (512MB disk space)
	- Recommended: initialize DB connections outside of function handler, it can be reused for next execution
	- For permanent persistence use S3 (not **/tmp**)!
- Concurrency:
	- Limit: up to 1000 concurrent executions per account
	- Reserved concurrency creates a pool of requests that can only be used by its function, and also prevents its function from using unreserved concurrency
	- Invocation over concurrency limit will trigger throttle
	- Throttle behavior:
		- Sync Invocation => return Throttle Error 429
		- Async Invocation => automatic retry and DLQ
	- If higher limit is required per account, open a support ticket
	- In case of reserved concurrency not set, one function could throttle other function, max concurrency limit is applied globally to a single account
	- In case of async invocation Lambda will attempt to run the function again for 6 hours. The retry interval increases exponentially
	- Cold start: in case of a new function instance, first request can take longer because of initialization
	- Provisioned concurrency initializes a requested number of execution environments so that they are prepared to respond to your function's invocations => no cold start!
	- Application Auto Scaling can manage concurrency (schedule or target utilization)

## External Dependencies

- Dependencies, packages need to be installed together with Lambda code (zip package)
- Zip can be uploaded to Lambda if it has less than 50MB. Otherwise, use S3
- Native libraries need to be compiled on Amazon Linux
- AWS SDK comes together with every Lambda function

## CloudFormation

- Code can be defined in-line (for very simple functions)
- Inline functions can not include dependencies
- For more complex functions:
    - We can use the **Code.ZipFile** property
    - We can use S3 where we should refer to the location of the zip file in a bucket
        - Note: if we update the code in S3 but don't update the S3Bucket, S3Key or S3ObjectVersion in the template, CloudFormation wont update the function, **versioning is import**

## Layers
- Custom Runtimes for Lambda, example:
    - C++ [https://github.com/awslabs/aws-lambda-cpp](https://github.com/itsmostafa/certified-aws-developer-associate-notes/blob/master/3-aws-serverless/aws-lambda-cpp)
    - Rust [https://github.com/awslabs/aws-lambda-rust-runtime](https://github.com/itsmostafa/certified-aws-developer-associate-notes/blob/master/3-aws-serverless/aws-lambda-rust)
- Externalize dependencies to re-use them in case of bigger dependency packages
- Another function could reuse layers

## Versions and Aliases

- When we work on a Lambda function, we work on _$LATEST_ version
- When we are ready to publish, we create a new version
- Versions are immutable (can not change to code, env. variables of anything after a version is published)
- Versions have increasing number
- Each version is independent and it gets its own ARN
- Each version of the lambda can be accessed
- Alias:
    - Pointer to a Lambda function version
    - We can define dev, test, prod aliases which can point to different versions
    - Aliases are mutable
    - Aliases enable Blue/Green deployment by assigning weights to Lambda functions
    - Aliases enable stable configurations of our event triggers/destinations
    - Aliases have their own ARN
    - **Aliases can not reference other aliases!**
## Limits

- Execution:
    - Memory: 128MB - 3008MB (64MB increments)
    - Max execution time: 900 seconds (15 minutes)
    - Env. variables: 4KB
    - Disk space (/tmp): 512MB
    - Concurrent executions 1000 per account
- Deployments:
    - Compressed: 50MB
    - Uncompressed (code + dependencies): 250MB
    - Can use /tmp to load other files at startup
## Best practices

- Perform heavy-duty work outside the function handler
- Use env. variables for anything that changes over time
- Minimize deployment package size to its runtime necessities
- **Avoid recursive code, never have a Lambda function call itself!**

## Docker images

- You can now package and deploy Lambda functions as container images of up to 10 GB in size.
- AWS provides you with the base images, and you add your code/dependencies
- You can create your own images implementing the Lambda runtime API
- Can be used to deploy large workloads (like machine learning or data intensive workloads)