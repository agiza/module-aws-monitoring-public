**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-aws-monitoring>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-aws-monitoring/blob/master/modules/logs/cloudwatch-log-aggregation-iam-policy/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# CloudWatch Log Aggregation IAM Policy

This module contains Terraform templates that define an [IAM
policy](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/QuickStartEC2Instance.html#d0e22325) required
for CloudWatch Logging. You can attach this IAM policy to any EC2 instances that need to send their logs to CloudWatch.
See the [cloudwatch-log-aggregation-scripts module](../cloudwatch-log-aggregation-scripts) for scripts you can use to
configure your EC2 instances to do CloudWatch Log Aggregation.

## Example

See the [cloudwatch-log-aggregation example](/examples/cloudwatch-log-aggregation) for an example of how to use this
module.

## How do you use this module?

To set up CloudWatch Log Aggregation, you need to do two things:

1. Run the CloudWatch Logs Agent on your EC2 instances
2. Provide your EC2 instances with an IAM policy

Both are described next.

#### Run the CloudWatch Logs Agent on your EC2 instances

To send log information to CloudWatch, you should use the [CloudWatch Logs
Agent](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/QuickStartEC2Instance.html). The best way to
install it on your EC2 instances is to use the [cloudwatch-log-aggregation-scripts
module](../cloudwatch-log-aggregation-scripts), which have scripts for installing and running the CloudWatch Logs
Agent.

#### Provide your EC2 instances with an IAM policy

To allow your EC2 instances to talk to CloudWatch Logs, you need the right IAM policy. The Terraform templates in this
module create this policy up for you. You just need to use an
[aws_iam_policy_attachment](https://www.terraform.io/docs/providers/aws/r/iam_policy_attachment.html) to attach that
policy to the IAM roles of your EC2 instances.

```hcl
module "cloudwatch_log_aggregation" {
  source = "git::git@github.com:gruntwork-io/module-aws-monitoring.git//modules/logs/cloudwatch-log-aggregation-iam-policy?ref=v0.0.18"
}

resource "aws_iam_policy_attachment" "attach_cloudwatch_log_aggregation_policy" {
    name = "attach-cloudwatch-log-aggregation-policy"
    roles = ["${aws_iam_role.my_ec2_instance_iam_role.id}"]
    policy_arn = "${module.cloudwatch_log_aggregation.cloudwatch_log_aggregation_policy_arn}"
}
```