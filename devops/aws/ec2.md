# EC2 Instances

EC2 instances are a scalable web service that provide virtual machines that can run any needed software.

## Connecting

EC2 instances can be interacted with an SSH command.

`ssh -i ~/.ssh/aws.pem <username>@<Public IPv4 address>`

Typical user name will be `ubuntu` or `es2-user`.

If the pem file is too permissive you will need to `chmod 600 <file address>` and `chmod 700 <file address>` before you can connect via SSH.

## Serving a Web Page

To access a web application you have setup on your EC2 instance you need to make sure you have a TCP connection with a port range that includes the port your application is running. Then you go type <IPv4 address>:<port number> in your browser's URL bar to access.
