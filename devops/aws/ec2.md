# EC2 Instances

EC2 instances are a scalable web service that provide virtual machines that can run any needed software.

## Connecting

EC2 instances can be interacted with an SSH command.

`ssh -i ~/.ssh/aws.pem <username>@<Public IPv4 address>`

Typical user name will be `ubuntu` or `es2-user`.

If the pem file is too permissive you will need to `chmod 600 <file address>` and `chmod 700 <file address>` before you can connect via SSH.

