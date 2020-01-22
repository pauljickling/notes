# Terraform

Terraform is a so-called "infrastructure as code" tool. More concretely, it is used to manage environments and configurations for deployment. Terrafrom is platform agnostic, and manages state.

## Installation/Setup

Unzip file and run `terraform`. Make sure `terraform` binary is available in `PATH`. You can run `terraform` without any arguments to see if it is working.

`terraform init` in the root project directory after you have created your config file to download the necessary plugins.

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

Once this information is completed you can run `terraform apply` and the instance will be created.
