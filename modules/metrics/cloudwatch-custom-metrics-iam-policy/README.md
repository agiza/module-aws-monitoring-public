**Note**: This public repo contains the documentation for the private GitHub repo <https://github.com/gruntwork-io/module-aws-monitoring>.
We publish the documentation publicly so it turns up in online searches, but to see the source code, you must be a Gruntwork customer.
If you're already a Gruntwork customer, the original source for this file is at: <https://github.com/gruntwork-io/module-aws-monitoring/blob/master/modules/metrics/cloudwatch-custom-metrics-iam-policy/README.md>.
If you're not a customer, contact us at <info@gruntwork.io> or <http://www.gruntwork.io> for info on how to get access!

# CloudWatch Custom Metrics IAM Policy

This module contains Terraform templates that define an [IAM
policy](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/QuickStartEC2Instance.html#d0e22325) required
for reading and writing CloudWatch metrics. You can attach this IAM policy to any EC2 instances that need CloudWatch
metrics.

Note: CloudWatch provides many metrics for your EC2 instances by default, but not memory and disk usage metrics. See the
[cloudwatch-memory-disk-metrics-scripts module](../cloudwatch-memory-disk-metrics-scripts) for scripts you can use to
configure your EC2 instances to report memory and disk usage metrics as well.

## Example

See the [cloudwatch-custom-metrics example](/examples/cloudwatch-custom-metrics) for an example of how to use this
module.

## How do you use this module?

The basic idea is to add the module to your Terraform templates and then to use an
[aws_iam_policy_attachment](https://www.terraform.io/docs/providers/aws/r/iam_policy_attachment.html) to attach the IAM
policy to the IAM roles of your EC2 instances.

```hcl
module "cloudwatch_metrics" {
  source = "git::git@github.com:gruntwork-io/module-aws-monitoring.git//modules/metrics/cloudwatch-custom-metrics-iam-policy?ref=v0.0.18"
}

resource "aws_iam_policy_attachment" "attach_cloudwatch_metrics_policy" {
    name = "attach-cloudwatch-metrics-policy"
    roles = ["${aws_iam_role.my_ec2_instance_iam_role.id}"]
    policy_arn = "${module.cloudwatch_metrics.cloudwatch_metrics_policy_arn}"
}
```