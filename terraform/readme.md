# Terraform Standard Template Construct
## Content

- [1. Commands](#1-commands)
- [2. Files Basics](#2-files-basics-bookcontent)
- [3. Language Basics](#3-language-basics-bookcontent)
  - [3.1 Blocks](#31-blocks-bookcontent)
  - [3.2 Meta-Arguments](#32-meta-arguments-bookcontent)
- [4. Variables](#4-variables-bookcontent)
  - [4.1 Input](#41-input-bookcontent)
  - [4.2 Output](#42-output-bookcontent)
- [5. Data Sources](#5-data-sources-bookcontent)
- [6. Terraform Settings](#6-terraform-settings-bookcontent)
  - [6.1 Remote State with AWS S3](#61-remote-state-with-aws-s3-bookcontent)
  
## 1. Commands

[:book:](#content)

Basic pipeline:

```
terraform init -> terraform plan -> terraform apply
```

```
  init          Prepare your working directory for other commands
  validate      Check whether the configuration is valid
  plan          Show changes required by the current configuration
  apply         Create or update infrastructure
  destroy       Destroy previously-created infrastructure
  console       Try Terraform expressions at an interactive command prompt
  fmt           Reformat your configuration in the standard style
  output        Show output values from your root module
  taint         Mark a resource instance as not fully functional
  untaint       Remove the 'tainted' state from a resource instance
  import        Associate existing infrastructure with a Terraform resourceion
  workspace     Workspace management
```

## 2. Files Basics [:book:](#content)

All the files that have a `.tf` extension and where `terraform init` is run will create a Root Module (Note: there exist a json-based variant of the language for terraform).

Once `terraform plan` or `terraform apply` it will create a `terraform.tfstate` file which will hold the state of all your deployed resources. (See more Remote State)

## 3. Language Basics [:book:](#content)

### 3.1 Blocks [:book:](#content)

```
resource "aws_instance" "example" {
  ami = "abc123"

  network_interface {
    # ...
  }
}


```
Where:
- `aws_instance` is the Resource Type = <provider>_<provider_resource>
- `example` is Resource Name
- `ami` is an argument for the resource

You can make a reference to a resource by using an Identifier `<provider>_<provider_resource>.<resource_name>.<output_variable>` (E.g.: `aws_instance.example.id`)

### 3.2 Meta-arguments [:book:](#content)

- `depends_on` for handling resource inter-dependencies
```
resource "aws_iam_role_policy" "example" {
  name   = "example"
  role   = aws_iam_role.example.name
  # Insert other code here
}

resource "aws_instance" "example" {
  ami           = "ami-a1b2c3d4"
  # Insert other code here
  depends_on = [
    aws_iam_role_policy.example,
  ]
}
```
- `count` used to create multiple instances of an object (it uses a list under the hood)

```
resource "aws_instance" "server" {
  count = 4 # create four similar EC2 instances

  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"

  tags = {
    Name = "Server ${count.index}"
  }
}
```
- `for_each` used also for creating multiple instances but it uses a map/set for managing the resources created
```
resource "azurerm_resource_group" "rg" {
  for_each = {
    a_group = "eastus"
    another_group = "westus2"
  }
  name     = each.key
  location = each.value
}
resource "aws_iam_user" "the-accounts" {
  for_each = toset( ["Todd", "James", "Alice", "Dottie"] )
  name     = each.key
}
```
- `lifecycle` used for managing the lifecycle of resources
```
resource "azurerm_resource_group" "example" {
  # ...

  lifecycle {
    create_before_destroy = true
  }
}
```
- Types
    - `create_before_destroy`: bool
    - `prevent_destroy`: bool
    - `ignore_changes`: (list of attribute names) 

- `provider` allows for configuration of a specific provider

```
# default configuration
provider "google" {
  region = "us-central1"
}

# alternate configuration, whose alias is "europe"
provider "google" {
  alias  = "europe"
  region = "europe-west1"
}

resource "google_compute_instance" "example" {
  # This "provider" meta-argument selects the google provider
  # configuration whose alias is "europe", rather than the
  # default configuration.
  provider = google.europe

  # ...
}
```

## 4. Variables [:book:](#content) 

### 4.1 Input [:book:](#content)

```
variable "availability_zone_names" {
  type    = list(string)
  default = ["us-west-1a"]
}

variable "image_id" {
  type = string
}

```

You can assign values either by passing the `default` parameter or through a file called `terraform.tfvars` or ending in `*.auto.tfvars` or by using environment variables (E.g.: `export TF_VAR_image_id=ami-abc123`)

- `Example for a .tfvars file`
```
image_id = "ami-abc123"
availability_zone_names = [
  "us-east-1a",
  "us-west-1c",
]

```

### 4.2 Output [:book:](#content)

Allows for expose information for other terraform configuration and offer data in the command line

- Uses Examples: 
```
Output values have several uses:

- A child module can use outputs to expose a subset of its resource attributes to a parent module.
- A root module can use outputs to print certain values in the CLI output after running terraform apply.
- When using remote state, root module outputs can be accessed by other configurations via a terraform_remote_state data source.

```

- Declaration Example:
```
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
  description = "The private IP address of the main server instance."
}
output "db_password" {
  value       = aws_db_instance.db.password
  description = "The password for logging in to the database."
  sensitive   = true
}
```

## 5. Data Sources [:book:](#content)
- Allows terraform to use information defined outside of terraform.
- It allows only to read infrastructure, not change it.

```
data "aws_ami" "example" {
  most_recent = true

  owners = ["self"]
  tags = {
    Name   = "app-server"
    Tested = "true"
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.web.id
  instance_type = "t1.micro"
}
```

## 6. Terraform Settings [:book:](#content)
Terraform settings are gathered together into terraform blocks, that allows for either specifing a specific version of a provider or even keeping the state in a remote `backend`.
```
terraform {
    required_providers {
    aws = {
      version = ">= 2.7.0"
      source = "hashicorp/aws"
    }
  }
}
```

### 6.1 Remote State with AWS S3 [:book:](#content)

You will need a:
- An S3 bucket (you will need its name)
- The path where you will store the `terraform.tfstate` inside the bucket
- An DynamoDB table (used for state locking: "making sure just one person modifies the state at a certain point in time")
    - The Primary Key/Hash Key with the name `LockID` with the type of `String`
- The region of your AWS resources

For more info see: https://www.terraform.io/language/settings/backends/s3

Example:

```
terraform {
    backend "s3" {
        bucket = "kodecloud-terraform-state-bucket01"
        key = "finance/terraform.tfstate"
        region = "us-west-1"
        dynamodb_table = "state-locking"
    }
}

```

Steps:
- add terraform backend inside a file called "terraform.tf" (good practice for holding configuration of terraform).
- do `terraform init` and `terraform apply` locally to initialize the state

## Functions and Conditional Expressions

## Modules