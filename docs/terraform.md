# Terraform

```
terraform init
```

Downloads and installs providers in `.terraform/` and creates a lock file
`.terraform.lock.hcl`

```
terraform apply
```

```
terraform destroy
```

Terminates resources managed by terraform project. The inverse of
`terraform apply`.

## Providers

Configure specified provider.

Find terraform providers in the terraform registry.

`main.tf`
```
provider "aws" {
  profile = "..."
  region  = "..."
}
```

## Resources

Define and configure components.

- `resource type` - prefix maps to the name of the provdier, eg `aws_instance`
- `resource name` 

The resouce type and resource name form a uniqe id for the resource as
`<resource_type>.<resource_name>`.

`main.tf`
```
resouces "<resource type>" "<resource name>" {
  ami = "..."
  instance_type = ""

  tags = {
    Name = "..."
  }
}
```

## State

Terraform uses the state to keep track of the resources it manages. The state
needs to be up to date.

File `terraform.tfstate`.

```
terraform show
```

```
terraform state list
```

**Note**: state stores in plain text, so it contains sensitive information.

### Backend - Remote State

Define where terraform's state snapshots are stored.

### S3

```
resource "aws_s3_bucket" "<backend name>" {
  bucket = "<globally unique bucket name>"

  lifecycle {
    prevent_destroy = true // prevents terraform to delete
  }

  versioning {
    enabled = true // allows us to see what the state was at a previous moment in time
  }

  server_side_encryption_configuration { // enable encryption of state file
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }
}
```

Use DynamoDB to enable locking of state in order to prevent more than one user
to edit the state at the same time.

```
resource "aws_dynamodb_table" "<terraform lock>" {
  name = "<dynamodb name>"
  billing_mode = "PAY_PER_REQUEST"
  hash_key = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }
}
```

Define state to use

```
terraform {
  backend "s3" {
    bucket = "<bucket name>"
    key = "global/s3/terraform.tfstate" // Folder path in bucket to store state file
    region = "<aws region>"
    dynamodb_table = "<dynamodb name>"
    encrypt = true
  }
}
```

**Remark**: The target backend needs to exist in order for terraform to be able
to use it.

## Variables

### Input

Terraform automatically loads variable values from any files that end in
`.tfvars`.

#### File `variables.tf`

`variables.tf`

```
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "ExampleAppServerInstance"
}
```

Use variables in `main.tf` by syntax `var.instance_name`.

#### CLI parameter

`terraform apply -var 'instance_name=AnotherName'`.

### Output

`outputs.tf`

Terraform outputs output variables to the screen on `terraform apply`.

Output variables can be queried with `terraform output`.

## Syntax

```
terraform fmt
```

```
terraform validate
```

## Terraform Cloud

Requires an account available with `terraform login`.

## Modules

A module is a directory containing `.tf` files. When running commands in that
directory it is considered the **root module**.

**Best practice**: Name your provider `terraform-<PROVIDER>-<NAME>`.

Install modules by using either `terraform get` or `terraform init`. Remote
plugins will be downloaded to the `.terraform` directory.

### Using a Module

Use a module with the `source` argument, it can be either

- Terraform Registry
- HTTP URLs
- local path
- GitHub
- S3 bucket
- Modules in Package Sub-directories

Declare a module inside a `module` block.

```
module "<a name>" {
  source = "./modules/<local path module>"

  <module input variables>
}
```

Modules have

- *required* and *optional* inputs
- outputs accessible by `module.<MODULE NAME>.<OUTPUT NAME>`

When Terraform processes a module block, it will inherit the provider from the
enclosing configuration.

**Best practice**: Do not include provider blocks in modules.

### Structure

**Best practice**:

```
.
├── LICENSE
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
├── modules
│   └── <module>
```

`variables.tf` will define inputs when used as a module. Variables without
a default value will become required inputs.

Don't distribute `terraform.tfstate`, `terraform.tfstate.backup`, `.terraform`,
`*.tfvars` (they contain sensitive information).

`LICENSE` and `README.md` are not used by Terraform.

### Separate States

Don't use `terraform apply -target x` to separate states.

Two methods:

- directories,
- workspaces.

#### Directories

```
├── prod
│   ├── main.tf
│   ├── variables.tf
│   ├── terraform.tfstate
│   └── terraform.tfvars
└── dev
    ├── main.tf
    ├── variables.tf
    ├── terraform.tfstate
    └── terraform.tfvars
```

#### Workspaces

Use same Terraform code but have different state files.

```
terraform workspace new <workspace name>
```

```
terraform apply -var-file=<environment variable file>.tfvars
```

```
terraform destroy -var-file=<environment variable file>.tfvars
```

```
terraform workspace list
```

```
terraform workspace select <environment>
```

Terraform workspace name is accessible with `terraform.workspace`.

