# EC2 Instances

EC2 instances are a scalable web service that provide virtual machines that can run any needed software.

## Connecting

EC2 instances can be interacted with an SSH command.

`ssh -i ~/.ssh/aws.pem <username>@<Public IPv4 address>`

Typical user name will be `ubuntu` or `es2-user`.

If the pem file is too permissive you will need to `chmod 600 <file address>` and `chmod 700 <file address>` before you can connect via SSH.

## Serving a Web Page

To access a web application you have setup on your EC2 instance you need to make sure you have a TCP connection with a port range that includes the port your application is running. You can configure this in the security group for your EC2 instance. Then you type `<IPv4 address>:<port number>` in your browser's URL bar to access.

## Tagging EC2 Resources

Tags are metadata labels associated with each resource using key/(optional)value pairs. Tags are returned by many different API calls, and therefore you should not include any sensitive data in your tags.

Examples of keys might be something like:

```
[resource1]
Owner=DbAdmin
Stack=Test

[resource2]
Owner=DbAdmin
Stack=Production
```

Tag values are strings so you can assign a tag an empty string value `""` but not `null`.

Resources can have up to 50 tags. Each key must be unique, and can only have a single value. The max key length is 128 chars, and the max value length is 256 chars. Key/value pairs *are* case-sensitive. Finally, `aws:` is a reserved key prefix.

### Tagging Resources for Billing

One important use of tagging is to itemize your resources for billing purposes. By default it is essentially impossible to have a cost breakdown per resource, but tags can provide a human-friendly readable and identifiable phrase that makes it possible to identify and group resources.

By default new tag keys are not included in the cost allocation report. To include them, the following steps need to be taken:

1. Open the Billing and Cost Management console.
2. From the navigation pane under **Preferences** select **Billing Preferences**.
3. In the **Report** list check the box for **Cost allocation report**.
4. Select **Manage report tags** and select the group of tags you want to include in the report, and click the **Activate** button.
