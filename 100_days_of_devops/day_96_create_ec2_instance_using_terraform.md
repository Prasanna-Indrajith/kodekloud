# Create EC2 Instance Using Terraform

## Question

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance using Terraform with the following requirements:

The EC2 instance must use the value datacenter-ec2 as its Name tag, which defines the instance name in AWS.

Use the Amazon Linux ami-0c101f26f147fa7fd to launch this instance.

The Instance type must be t2.micro.

Create a new RSA key named datacenter-kp.

Attach the default (available by default) security group.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to provision the instance.

## Answer

### 1. Create and configure main.tf

```js
# 1. Generate the RSA Private Key inside Terraform
resource "tls_private_key" "rsa_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# 2. Create the Key Pair in AWS using the generated key
resource "aws_key_pair" "datacenter-kp" {
  key_name   = "datacenter-kp"
  public_key = tls_private_key.rsa_key.public_key_openssh
}

# 3. Find the Default VPC and its Default Security Group
data "aws_vpc" "default" {
  default = true
}

data "aws_security_group" "default_sg" {
  vpc_id = data.aws_vpc.default.id
  name   = "default"
}

# 4. Create the EC2 Instance
resource "aws_instance" "datacenter-ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  key_name      = aws_key_pair.datacenter-kp.key_name

  # Attach the default security group
  vpc_security_group_ids = [data.aws_security_group.default_sg.id]

  tags = {
    Name = "datacenter-ec2"
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

Creating an **EC2 (Elastic Compute Cloud)** instance is the process of launching a virtual server in the AWS cloud. In Terraform, this is managed using the aws_instance resource.

To launch one, Terraform needs to know three main things: what OS to use (**AMI**), what size the server should be (**Instance Type**), and where to put it (**Subnet**).

```js
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0" # The "Image" (e.g., Ubuntu, Amazon Linux)
  instance_type = "t2.micro"             # The "Size" (CPU/RAM)

  # Optional
  subnet_id              = "subnet-12345" # Which network to join
  vpc_security_group_ids = ["sg-67890"]   # Which firewall rules to use

  tags = {
    Name = "Nautilus-App-Server"
  }
}
```
