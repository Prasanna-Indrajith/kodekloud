# Create IAM Policy Using Terraform

## Question

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements.

Create an IAM policy named `iampolicy_kirsty` in `us-east-1` region using Terraform. It must allow read-only access to the EC2 console, i.e., this policy must allow users to view all instances, AMIs, and snapshots in the Amazon EC2 console.

## Answer

### 1. Create and configure main.tf

```js
resource "aws_iam_policy" "iampolicy_kirsty" {
  name        = "iampolicy_kirsty"
  path        = "/"
  description = "Read-only access to EC2 console for instances, AMIs, and snapshots"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots",
          "ec2:DescribeTags"
        ]
        Effect   = "Allow"
        Resource = "*"
      }
    ]
  })
}

output "policy_arn" {
  value = aws_iam_policy.iampolicy_kirsty.arn
}
```

### 2. Execute

```bash
terraform init

terraform plan

terraform apply -auto-approve
```

## Additional Details

In Terraform, an **IAM Policy** is a JSON document that defines "who can do what" to which AWS resources.

While you can create a policy manually in the AWS Console, using Terraform allows you to version-control your permissions and reuse them across different environments.

### The Three Essential Steps

To fully implement an IAM Policy in Terraform, you typically follow these three steps:

- Step A: Define the Document – Create the JSON logic (the permissions).
- Step B: Create the Policy Resource – Turn that logic into a managed AWS Policy object.
- Step C: Attach the Policy – Link the policy to a User, Group, or Role.

```js
# 1. Define the permissions (Data Source)
data "aws_iam_policy_document" "s3_read_access" {
  statement {
    actions   = ["s3:GetObject", "s3:ListBucket"]
    resources = ["arn:aws:s3:::my-nautilus-bucket/*"]
    effect    = "Allow"
  }
}
```
