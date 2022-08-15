# Identity Access Management (IAM)

It has the following capabilities:
- It is a global service
- It allows for creating users and groups which you can assign policies.
- These policies define the permissions of the users
- Follow the least privilege principle (don't give more permissions that a user needs)

## Policies
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

## Roles

Allow services from AWS to perform actions on your behalf by granting permissions with IAM Roles.

## Security Tools

1. IAM Credentials Report: list all your account's users and the status of their various credentials
2. IAM Access Advisor: show service permissions granted to a user and when those services were last accessed.

## Best Practices

- Don't user root account except for aws account setup.
- One physical user = one AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce MFA
- Create and use roles for giving permissions to AWS services
- User access keys for programmatic access (CLI/SDK)
- Never share IAM users or access keys


## Shared Responsibility Model for IAM

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