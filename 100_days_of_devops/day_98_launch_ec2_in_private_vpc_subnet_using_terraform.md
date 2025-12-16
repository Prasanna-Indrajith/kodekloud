# Launch EC2 in Private VPC Subnet Using Terraform

## Question

The Nautilus DevOps team is expanding their AWS infrastructure and requires the setup of a private Virtual Private Cloud (VPC) along with a subnet. This VPC and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VPC. Additionally, the team needs to provision an EC2 instance under the newly created private VPC. This instance should be accessible only from within the VPC, allowing for secure communication and resource management within the AWS environment.

1. Create a VPC named `xfusion-priv-vpc` with the CIDR block `10.0.0.0/16`.

2. Create a subnet named `xfusion-priv-subnet` inside the VPC with the CIDR block `10.0.1.0/24` and `auto-assign` IP option must not be `enabled`.

3. Create an EC2 instance named `xfusion-priv-ec2` inside the subnet and `instance` type must be `t2.micro`.

4. Ensure the security group of the EC2 instance allows access only from within the VPC's CIDR block.

5. Create the main.tf file (do not create a separate .tf file) to provision the VPC, subnet and EC2 instance.

6. Use `variables.tf` file with the following variable names:

`KKE_VPC_CIDR` for the VPC CIDR block.
`KKE_SUBNET_CIDR` for the subnet CIDR block.

7. Use the `outputs.tf` file with the following variable names:

`KKE_vpc_name` for the name of the VPC.
`KKE_subnet_name` for the name of the subnet.
`KKE_ec2_private` for the name of the EC2 instance.

## Answer

### 1. Create main.tf

```js
# 1. Create the VPC
resource "aws_vpc" "xfusion-priv-vpc" {
  cidr_block = var.KKE_VPC_CIDR

  tags = {
    Name = "xfusion-priv-vpc"
  }
}

# 2. Create the Subnet (Auto-assign IP disabled)
resource "aws_subnet" "xfusion-priv-subnet" {
  vpc_id                  = aws_vpc.xfusion-priv-vpc.id
  cidr_block              = var.KKE_SUBNET_CIDR
  map_public_ip_on_launch = false

  tags = {
    Name = "xfusion-priv-subnet"
  }
}

# 3. Security Group (Allow ONLY internal VPC traffic)
resource "aws_security_group" "xfusion-sg" {
  name        = "xfusion-priv-sg"
  vpc_id      = aws_vpc.xfusion-priv-vpc.id

  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = [var.KKE_VPC_CIDR] # Restricted to VPC CIDR
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# 4. Create the EC2 Instance
resource "aws_instance" "xfusion-priv-ec2" {
  ami                    = "ami-0c101f26f147fa7fd" # Standard Amazon Linux
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.xfusion-priv-subnet.id
  vpc_security_group_ids = [aws_security_group.xfusion-sg.id]

  tags = {
    Name = "xfusion-priv-ec2"
  }
}
```

### 2. Create variables.tf

```js
variable "KKE_VPC_CIDR" {
  default = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
  default = "10.0.1.0/24"
}
```

### 3. Create outputs.tf

```js
output "KKE_vpc_name" {
  value = aws_vpc.xfusion-priv-vpc.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.xfusion-priv-subnet.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.xfusion-priv-ec2.tags["Name"]
}
```

### 4. Execute

```bash
terraform init

terraform plan

terraform apply -auto-approve
```

## Additional Details

Launching an EC2 instance in a Private Subnet means placing your server in a section of your network that has no direct path to the open internet.

This is a standard security practice for databases or internal application servers that shouldn't be publicly reachable.
