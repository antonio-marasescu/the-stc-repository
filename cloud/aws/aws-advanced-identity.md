# Advanced Identity

## Security Token Service (STS)

Enables to create temporary, limited privilges credential to access your AWS resources with short-term configurable expiration period.

Use Cases:
- Identity federation: manage user identities in external systems
- IAM Roles for Cross/Same account access
- IAM roles for EC2: temporary access to other aws resources

## AWS Cognito

Provides identity for your application users (web application, mobile application, etc.). 

**Note:** This is provided as the solution for not creating IAM users which should be used only by your organization/company and need to use AWS directly.
