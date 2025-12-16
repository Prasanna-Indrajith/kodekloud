# Create and Configure Alarm Using CloudWatch Using Terraform

## Question

The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization. The alarm should trigger if the CPU utilization exceeds 90% for one consecutive 5-minute period. To send notifications, use the SNS topic named devops-sns-topic, which is already created.

1. **Launch EC2 Instance**: Create an EC2 instance named `devops-ec2` using any appropriate `Ubuntu` AMI (you can use AMI `ami-0c02fb55956c7d316`).

2. **Create CloudWatch Alarm**: Create a CloudWatch alarm named `devops-alarm` with the following specifications:

Statistic: Average
Metric: CPU Utilization
Threshold: >= 90% for 1 consecutive 5-minute period
Alarm Actions: Send a notification to the devops-sns-topic SNS topic.

3. Update the `main.tf` file (do not create a separate .tf file) to create a EC2 Instance and CloudWatch Alarm.

4. Create an `outputs.tf` file to output the following values:

`KKE_instance_name` for the EC2 instance name.
`KKE_alarm_name` for the CloudWatch alarm name.

## Answer

### 1. Create main.tf

```js
# 1. The SNS Topic (Must be present for the alarm to reference it)
resource "aws_sns_topic" "sns_topic" {
  name = "nautilus-sns-topic"
}

# 2. The EC2 Instance
resource "aws_instance" "nautilus_node" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "nautilus-ec2"
  }
}

# 3. The CloudWatch Alarm
resource "aws_cloudwatch_metric_alarm" "nautilus_cpu_alarm" {
  alarm_name          = "nautilus-alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = "1"
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = "300"
  statistic           = "Average"
  threshold           = "90"

  dimensions = {
    InstanceId = aws_instance.nautilus_node.id
  }

  # This references the resource created at the top of this file
  alarm_actions = [aws_sns_topic.sns_topic.arn]
}
```

### 2. Create outputs.tf

```js
output "KKE_instance_name" {
  value = aws_instance.nautilus_node.tags.Name
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.nautilus_cpu_alarm.alarm_name
}
```

### 5. Execute

```bash
terraform init

terraform plan

terraform apply -auto-approve
```

## Additional Details

Creating a CloudWatch Alarm in Terraform allows you to automatically monitor your AWS resources and trigger actions (like sending an email or stopping a server) when certain thresholds are met.

In Terraform, this is primarily managed using the aws_cloudwatch_metric_alarm resource.
