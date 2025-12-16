# Create Security Group Using Terraform

## Question

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Use Terraform to create a security group under the default VPC with the following requirements:

1. The name of the security group must be `devops-sg`.

2. The description must be `Security group for Nautilus App Servers`.

3. Add an `inbound` rule of type `HTTP`, with a port range of `80`, and source `CIDR` range `0.0.0.0/0`.

4. Add another `inbound` rule of type `SSH`, with a port range of `22`, and source `CIDR` range `0.0.0.0/0`.

Ensure that the security group is created in the `us-east-1` region using Terraform. The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different .tf file) to accomplish this task.

## Answer

### 1. Create and configure main.tf

Because of all `providers.tf` already created and cofigured to have region `us-esat-1`, We only have to configure `Resources` only inside `main.tf` file.

```js
#Default aws_vps
data "aws_vpc" "default" {
  default = true
}

# Create security group
resource "aws_security_group" "devops-sg" {
  name        = "devops-sg"
  description = "Security group for Nautilus App Servers"

  # 1. Inbound Rules (Traffic coming IN)
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

### 2. Execute

```bash
terraform init

terraform plan

terraform apply -auto-approve
```

## Additional Details

In Terraform (and AWS), a Security Group acts as a **virtual firewall** for your instances (VMs). It controls the traffic that is allowed to enter (Ingress) and leave (Egress) your resources.

```js
resource "aws_security_group" "web_sg" {
  name        = "allow_web"
  description = "Allow HTTP and SSH traffic"
  vpc_id      = aws_vpc.devops-vpc.id # Links it to your VPC

  # 1. Inbound Rules (Traffic coming IN)
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] # Allow anyone from the internet
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["1.2.3.4/32"] # Best practice: Allow only your specific IP
  }

  # 2. Outbound Rules (Traffic going OUT)
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"          # "-1" means ALL protocols
    cidr_blocks = ["0.0.0.0/0"] # Let the server talk to the whole internet
  }
}
```
