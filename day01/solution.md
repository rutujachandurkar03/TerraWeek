# TerraWeek Day 1: Introduction to Terraform and Terraform Basics

Welcome to **Day 1** of TerraWeek! Today, we begin our Terraform journey by understanding the fundamentals of this powerful Infrastructure as Code (IaC) tool and using it to create infrastructure on AWS. 

## What is Terraform?
Terraform, developed by HashiCorp, is an open-source IaC tool that allows you to define, provision, and manage infrastructure across cloud providers like AWS, Azure, and GCP.

### Why use Terraform?
- Automates infrastructure provisioning, reducing manual errors.
- Supports multi-cloud setups seamlessly.
- Tracks infrastructure state for easy rollbacks and updates.
- Enables modular, reusable configurations for scalability.

---

## What I Built Today
Using Terraform, I created the following on AWS:

1. A key pair for secure SSH access.
2. A security group allowing SSH (port 22) and HTTP (port 80) traffic.
3. A default VPC.
4. An EC2 instance connected using the generated key pair.

---

## Terraform Configuration Files

### `terraform.tf`
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.78.0"
    }
  }
}

provider "aws" {
  region = "eu-west-1"
}
```

### `ec2.tf`
```hcl
resource "aws_key_pair" "my_key" {
  key_name   = "keypair-key"
  public_key = file("keypair-key.pub")
}

resource "aws_default_vpc" "default" {}

resource "aws_security_group" "my_sg" {
  name        = "my z plus security"
  description = "this is a SG created by TF"
  vpc_id      = aws_default_vpc.default.id

  ingress {
    description = "allow access to ssh port 22"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "allow http port 80"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    description = "allow all outgoing traffic"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    name = "my z plus security"
  }
}

resource "aws_instance" "my_instance" {
  instance_type   = "t2.micro"
  security_groups = [aws_security_group.my_sg.name]
  key_name        = aws_key_pair.my_key.key_name
  ami             = "ami-0d64bb532e0502c46"

  root_block_device {
    volume_size = 10
    volume_type = "gp3"
  }

  tags = {
    name = "my-server"
  }
}
```

---

## Terraform Steps
1. **Initialize Terraform:**
   ```bash
   terraform init
   ```

2. **Plan the Infrastructure:**
   ```bash
   terraform plan
   ```

3. **Apply the Configuration:**
   ```bash
   terraform apply
   ```

---

## Key Terraform Terminologies
- **Provider:** Connects Terraform to cloud platforms like AWS, Azure, or GCP.
- **Resource:** Represents infrastructure components such as EC2 instances or VPCs.
- **State:** Tracks the current state of the infrastructure.
- **Module:** A reusable set of Terraform configurations.
- **Variable:** Parameterizes configurations for reusability.

---

