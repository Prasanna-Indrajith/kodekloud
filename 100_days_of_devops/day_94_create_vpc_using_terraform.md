# Create VPC Using Terraform

## Question

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a VPC named `devops-vpc` in region `us-east-1` with any `IPv4` CIDR block through terraform.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

## Answer

### 1. Create and configure main.tf

Because of all `providers.tf` already created and cofigured to have region `us-esat-1`, We only have to configure `Resources` only inside `main.tf` file.

```Terraform
resource "aws_vpc" "devops-vpc" {
    cidr_block = "10.0.0.0/16"

    tags = {
        Name = "devops-vpc"
    }
}
```

### 2. Execute

```bash
terraform init

terraform plan

terraform apply -auto-approve
```

## Brief Description About the Environment

### Terraform

Terraform is the king of Infrastructure as Code (IaC) (creating the actual servers, networks, and load balancers).

1. **The Core Concept**

   Terraform looks at what currently exists, compares it to your code, and only changes what is necessary.

   > Terraform builds the house, and Ansible paints the walls.

2. **Fundemental Blocks**

   A. The Provider
   B. The Resource
   C. Variables

#### A. The Provider

```js
provider "aws" {
  region = "us-east-1"
}
```

#### B. The Resource

```js
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "Nautilus-Web"
  }
}
```

#### C. Variables

```js
variable "server_port" {
  description = "Port for HTTP requests"
  default     = 80
}
```

3. **The Terraform Lifecycle (The Workflow)**

   To run Terraform, you almost always follow these four commands in order:

   - `terraform init`: Initializes the directory and downloads the necessary providers.

   - `terraform plan`: The "preview" step. It shows you exactly what it will create, change, or destroy without actually doing it.

   - `terraform apply`: Executes the plan. It asks for your "yes" before making changes.

   - `terraform destroy`: Deletes everything managed by that specific configuration.
