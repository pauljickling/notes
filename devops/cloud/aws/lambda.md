# AWS Lambda

The AWS Lambda service is a way to run code without provisioning servers. To create a Lambda service you can select the Lambda console, and from there click the "Create function" button.

From there you have the option to create a function from scratch, use a template function (called "Use a blueprint"), or select a sample Lambda app from a repo.

You will need to assign a name to your function, an IAM role, an execution role, and a policy template. Once you have assigned those values, you can select the "Create function" button.

This will take you to the configuration panel. Notably you can specify things like the runtime environment (i.e. what scripting language is being used), and the "handler" which is essentially the file name that is called by the main script. There are also configuration values for memory, timeouts, and VPC settings, among other things.

To invoke your function in a test environment you can go to **Configure Test Event** from the top of the page which will create a popup that allows you to further configure the test input data for your function. You can click create, and then run the test on your function at which point you can examine the logs to review the output.
