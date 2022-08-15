# Amazon Web Services (AWS)

## Cloud Computing

Providing capabilities for a developer to don't manage his infrastructure at home/work and only focus on development.

- **IaaS** (Infrastructure as a Service) 
  - provides building blocks for cloud IT
  - provides networking, computers and data storage space
  - Note: basically you manage everything but computers, but you manage the OS running on the computers (e.g.: EC2)
- **PaaS** (Platform as a Service)
  - Removes the need for your organization to manage underlying infrastructure
  - Focus on deployment and management of your application
  - Note: basically you manage only the application and data, but you don't manage the OS (e.g.: Elastic Beanstalk)
- **SaaS** (Software as a Service)
  - is a completed product that is run and managed by a service provider
  - Note: you directly interact through an application for every feature (e.g.: Rekognition, Dropbox, Gmail, etc.)

## Identity Access Management (IAM)

It has the following capabilities:
- It is a global service
- It allows for creating users and groups which you can assign policies.
- These policies define the permissions of the users
- Follow the least privilege principle (don't give more permissions that a user needs)

### Policies
One or more IAM Policies can be assigned either to a *User Group* or to a individual *User*.

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





