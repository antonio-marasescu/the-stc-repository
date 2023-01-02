# Identity Access Management (IAM) & Advanced Identity & Organizations

## 1. IAM

It has the following capabilities:
- It is a global service
- It allows for creating users and groups which you can assign policies.
- These policies define the permissions of the users
- Follow the least privilege principle (don't give more permissions that a user needs)

### 1.1. Policies
Represent json documents which define users, groups or roles permissions.
One or more IAM Policies can be assigned either to a *User Group* or to an individual *User*.

A Policy is structure in the following way:
- Version: policy language version
- ID (Optional): identifier of that policy
- Statement (one or more required individual statements):
    - Sid (optional): identified of that statement
    - Effect: whether the statement allows or denies access ("Allow" or "Deny")
    - Principal: account/user/role to which this policy is applied to.
    - Action: list of actions this policy allows or denies
    - Resource: list of resources to which the actions are applied to.
    - Condition: conditions for when this policy is in effect (optional).

## 1.2. Roles

Allow services from AWS to perform actions on your behalf by granting permissions with IAM Roles.

## 1.3. Security Tools

1. IAM Credentials Report: list all your account's users and the status of their various credentials
2. IAM Access Advisor: show service permissions granted to a user and when those services were last accessed.

## 1.4. Best Practices

- Don't use root account except for aws account setup.
- One physical user = one AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce MFA
- Create and use roles for giving permissions to AWS services
- User access keys for programmatic access (CLI/SDK)
- Never share IAM users or access keys


## 1.5. Shared Responsibility Model for IAM

AWS is responsible for:
- Infrastructure (global network security)
- Configuration and vulnerability analysis
- Compliance validation

You are responsible for:
- Creating groups, roles, policies and users (management and monitoring of such things)
- Enabling MFA on all accounts
- Rotate all your keys often
- Use IAM tools to apply appropriate permissions
- Analyze access patterns and review permissions

## 2. Security Token Service (STS)

Allows the creation of temporary, limited-privileges credentials for AWS resources. This are short-term credentials with an expiration period you can configure.

**Use Case**
- IAM Roles for EC2: provide temporary roles for EC2 instances to access other AWS resources
- IAM Roles for cross/same account access
- Identity Federation: manage user identities in external systems and provide STS tokens for AWS resource access

## 3. AWS Directory Service 

Offers different services to connect to Microsoft Active Directory (AD) (a windows server based centralized security management and database of objects (users, accounts, computers, printers, etc.))

There are different solutions provided:
1. AWS Managed Microsoft AD: create your own AD in AWS and manage users. Will allow you to establish "trust" connection with your on-premise AD. Support MFA.
2. AD Connector: a gateway/proxy to redirect to a on-premise AD (the users are managed in the on-premise AD).
3. Simple AD: an managed AD directory in cloud in AWS (basically they created their own AD). Cannot be joined with an on-premise AD.

## 4. AWS IAM Identity Center (Formerly AWS Single Sign-On)

Grants ability to login with a single-login for your:
- AWS accounts inside your AWS Organizations
- Business Cloud Applications (e.g.: Salesforce, Microsoft 365, etc.)
- SAML2.0-enabled application
- EC2 Windows Instances

Moreover, it handles where the user data is stored either
- through the Built-in identity store in IAM Identity Center
- through 3rd party provider, e.g.: Active Directory, OneLogin, Okta, etc.

## 5. Organizations

It is a **global service** which allows for the management of multiple AWS accounts.
Provides a main account refered to as a "master account".

The benefits of using it are the following:
- Consolidated Billing for all accounts - single payment method (just one bill for all the costs)
- Pricing benefits from aggregated usage (volume discount for EC2, S3, etc.)
- Reserved Instances of EC2 are shared for all accounts (Pooling of Reserved EC2 Instances) (in case one account does not use it).
- Offers an API to automate AWS account creation.
- Allows you to restrict account privilieges using Service Control Policies (SCP)

### Multi-Account Strategies


The idea behind this is to create accounts for different groups (per departament, per cost-center, per dev/test/prod) based on regulatory restrictions using Service Control Policies (SCP) for better resource isolation (ex: VPC), to have separate per-account service limits and isoated account for logging.

Therefore you can do the following to better manage your different accounts:
- Have a Multi Account vs One Account with Multi Virtual Private Cloud (VPC)
- Use tagging standards for billing purposes
- Enable CloudTrail on all account and send logs to a central S3 account
- Send CloudWatch Logs to central logging account

#### Service Control Policies (SCP)


- Allows you to whitelist or blacklist IAM actions, it is applied at the Organizational Unit or account level, but it is not applied to the master account.
- It itself is applied to all users and roles of the account (including Root), but it is not applied to service-linked roles (they enable other aws services to integrate with aws organizations).
- Must have an explicit Allow (does not allow anything by default)
- Example: restrict access to certain services (e.g.: EC2, EMR), enforce PCI compliance.

#### Hierachy
```
 Root OU
     Master Account
        Prod OU
            Account A
                HR OU
                    Account B
                Finance OU
                    Account C
```








