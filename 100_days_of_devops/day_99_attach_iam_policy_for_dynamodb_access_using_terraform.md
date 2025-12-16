# Attach IAM Policy for DynamoDB Access Using Terraform

## Question

The DevOps team has been tasked with creating a secure DynamoDB table and enforcing fine-grained access control using IAM. This setup will allow secure and restricted access to the table from trusted AWS services only.

As a member of the Nautilus DevOps Team, your task is to perform the following using Terraform:

1. **Create a DynamoDB Table**: Create a table named `datacenter-table` with minimal configuration.

2. **Create an IAM Role**: Create an IAM role named d`atacenter-role` that will be allowed to access the table.

3. **Create an IAM Policy**: Create a policy named `datacenter-readonly-policy` that should grant read-only access (GetItem, Scan, Query) to the specific DynamoDB table and attach it to the role.

4. Create the `main.tf` file (do not create a separate .tf file) to provision the table, role, and policy.

5. Create the `variables.tf` file with the following variables:

`KKE_TABLE_NAME`: name of the DynamoDB table
`KKE_ROLE_NAME`: name of the IAM role
`KKE_POLICY_NAME`: name of the IAM policy

6. Create the `outputs.tf` file with the following outputs:

`kke_dynamodb_table`: name of the DynamoDB table
`kke_iam_role_name`: name of the IAM role
`kke_iam_policy_name`: name of the IAM policy

7. Define the actual values for these variables in the `terraform.tfvars` file.

8. Ensure that the `IAM policy` allows only read access and restricts it to the specific `DynamoDB table` created.

## Answer

### 1. Create main.tf

```js
# 1. Create DynamoDB Table
resource "aws_dynamodb_table" "datacenter_table" {
  name           = var.KKE_TABLE_NAME
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "LockID" # Minimal config requirement

  attribute {
    name = "LockID"
    type = "S"
  }
}

# 2. Create IAM Role (with Trust Policy for EC2/Service access)
resource "aws_iam_role" "datacenter_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      }
    ]
  })
}

# 3. Create Read-Only IAM Policy restricted to the table ARN
resource "aws_iam_policy" "datacenter_policy" {
  name = var.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "dynamodb:GetItem",
          "dynamodb:Scan",
          "dynamodb:Query"
        ]
        Effect   = "Allow"
        Resource = aws_dynamodb_table.datacenter_table.arn
      }
    ]
  })
}

# 4. Attach Policy to Role
resource "aws_iam_role_policy_attachment" "attach_readonly" {
  role       = aws_iam_role.datacenter_role.name
  policy_arn = aws_iam_policy.datacenter_policy.arn
}
```

### 2. Create variables.tf

```js
variable "KKE_TABLE_NAME" {
  type = string
}

variable "KKE_ROLE_NAME" {
  type = string
}

variable "KKE_POLICY_NAME" {
  type = string
}
```

### 3. Create terraform.tfvars

```js
KKE_TABLE_NAME = "datacenter-table";
KKE_ROLE_NAME = "datacenter-role";
KKE_POLICY_NAME = "datacenter-readonly-policy";
```

### 4. Create outputs.tf

```js
output "kke_dynamodb_table" {
  value = aws_dynamodb_table.datacenter_table.name
}

output "kke_iam_role_name" {
  value = aws_iam_role.datacenter_role.name
}

output "kke_iam_policy_name" {
  value = aws_iam_policy.datacenter_policy.name
}
```

### 5. Execute

```bash
terraform init

terraform plan

terraform apply -auto-approve
```

## Additional Details

Attaching an IAM policy for DynamoDB access involves granting an identity (like an EC2 instance or a User) specific permissions to perform actions such as `PutItem`, `GetItem` or `Query` on your database tables.

In Terraform, this is typically done by creating a policy with the necessary DynamoDB actions and then "linking" it to the resource using an Attachment resource.
