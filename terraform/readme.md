# Terraform Standard Template Construct

## Content

## Commands

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

## Files Basics

All the files that have a `.tf` extension and where `terraform init` is run will create a Root Module (Note: there exist a json-based variant of the language for terraform).

Once `terraform plan` or `terraform apply` it will create a `terraform.tfstate` file which will hold the state of all your deployed resources. (See more Remote State)

## Language Basics

### Blocks

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

You can make a reference to a resource by using an Identifier "<provider>_<provider_resource>.<resource_name>.<output_variable> (E.g.: `aws_instance.example.id`)

### Meta-arguments 

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

## Variables 

### Input

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

### Output

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


## Remote State