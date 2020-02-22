# Terraform

Terraform is a so-called "infrastructure as code" tool. More concretely, it is used to manage environments and configurations for deployment. Terrafrom is platform agnostic, and manages state.

## Installation/Setup

Unzip file and run `terraform`. Make sure `terraform` binary is available in `PATH`. You can run `terraform` without any arguments to see if it is working.

`terraform init` in the root project directory after you have created your config file to download the necessary plugins.

`terraform fmt` is recommended to enable standardization by automatically updating configurations in the current working directory with a consistent style/formatting.

`terraform validate` isa used to ensure that your configuration file is syntactically valid

`terraform plan` to verify the creation process of the application deployment.

## The Config File

example of a sample hcl (the unfortunately named HashiCorp Configuration Language) file that creates an aws instance:

```
provider "aws" {
  profile    = "default"
  region     = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-2757f631"
  instance_type = "t2.micro"
}
```

The `profile` attribute will refer to your AWS config file that is located in `~/.aws/credentials`.

You can verify your AWS profile credentials by running `aws configure`.

Note that the key/value pairs for the resource hashmap are required. Also pay close attention to the ami value. Those are region specific.

You can view version constraints of the providers in a configuration with the `terraform providers` command.

## Applying Terraform State

Once this information is completed you can run `terraform apply` and the instance will be created. Terraform will have also written some data to `terraform.tfstate`, which is a file that tracks the IDs of created resources.

The current state of the instance can be viewed with `terraform show`.

`terraform state` can be used for advanced state management, and `terraform state list` provides the list of resources and addresses that can be modified.

### Reading Terraform Plans

After running `terraform plan` or `terraform apply` you will view a list of actions that terraform will take in order to achieve the state described in your config file.

A `+` or `-` prefix means that Terraform will need to destroy and recreate a resource, whereas a `~` means the resource can be modified in place.

## Destroying Resources

Resources can be destroyed with the `terraform destroy` command.

## Provisioners

Provisioners are used after you have created resources to perform actions like upload files, run shell scripts, or run configuration management tools.

## Variables

### Input Variables

Instead of hard coding values, it is possible to provide input variables.

```
provider "aws" {
  region = var.region
}
```

Now the region is specified via a parameter that can be provided either in the command line with the `-var` flag, from a file (`terraform.tfvars`), or from environmental variables that take the form `TF_VAR_{variable name}`. Note that environmental variables can only be used for string-type variables.

### Output Variables

Output variables allow a user to control the type of output they are interested in. For example:

```
output "ip" {
  value = aws_eip.ip.public_ip
}
```

can be used to view the public ip address for the populated resources.
