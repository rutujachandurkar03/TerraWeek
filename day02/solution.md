Terraform Project: AWS & Docker Automation
Overview
This project demonstrates the use of Terraform to automate the deployment of AWS resources, configure Docker on an EC2 instance, and manage infrastructure as code (IaC). It includes the setup of an AWS EC2 instance, the installation of Docker, and the deployment of a Docker container running NGINX.

Tasks Covered
Familiarized with HCL Syntax

Explored HCL blocks, parameters, and arguments.
Created and managed Terraform resources like aws_instance, aws_security_group, and local_file.
Working with Variables and Data Types

Defined variables to manage dynamic input values (e.g., AWS region, Docker image name, key pair paths).
Used variables in main.tf and variables.tf for flexible configuration.
Terraform Configuration Practice

Added required_providers for AWS and Docker.
Used Terraform CLI commands (terraform init, terraform plan, terraform apply) to test and apply configurations.
Project Structure
bash
Copy code
terraform-project/
├── main.tf             # Core Terraform configuration (EC2, Docker setup)
├── variables.tf        # Variables definitions for dynamic input
├── terraform.tf        # Provider setup and required version info
├── terraform.tfvars    # Optional values for variables (can be used to override defaults)
└── README.md           # Project documentation
Setup Instructions
Prerequisites
Before running Terraform, ensure you have the following:

Terraform installed on your machine.
AWS credentials set up using the AWS CLI or environment variables.
SSH key pair (public and private) for EC2 instance access.
Steps to Run the Project
Clone the repository:

bash
Copy code
git clone https://github.com/your-username/terraform-project.git
cd terraform-project
Initialize Terraform: This will download the required providers.

bash
Copy code
terraform init
Review the Plan: Preview the changes that will be applied.

bash
Copy code
terraform plan
Apply the Configuration: Apply the configuration to provision resources.

bash
Copy code
terraform apply
Terraform will prompt you to confirm. Type yes to proceed.

Access the EC2 Instance: Once the infrastructure is provisioned, Terraform will output the EC2 instance's public IP. You can SSH into the instance using the private key you specified in variables.tf.

bash
Copy code
ssh -i /path/to/your/private-key.pem ubuntu@<instance_public_ip>
Docker Container: Docker will be installed on the EC2 instance, and an NGINX container will be running. You can access it via the external port specified in the configuration.

Variables Configuration
aws_region: The AWS region where resources will be created (default: eu-west-1).
aws_key_name: The name of the AWS key pair for SSH access (default: keypair).
aws_public_key_path: The path to the public key file for the key pair.
aws_private_key_path: The path to the private key for SSH access.
docker_image_name: The Docker image to be used (default: nginx:latest).
docker_container_name: The name of the Docker container (default: nginx_container).
docker_port_external: The external port to expose the Docker container (default: 80).
docker_port_internal: The internal port for the Docker container (default: 80).
Example Output
After successful execution, Terraform will output the Public IP of the EC2 instance:

makefile
Copy code
Outputs:

instance_public_ip = "xx.xx.xx.xx"
You can access the NGINX web server running in the Docker container via the public IP on port 80.

Notes
Security: The security group configuration allows SSH (port 22) and HTTP (port 80) traffic from any IP. In a production environment, consider restricting the source IP ranges for security purposes.
SSH Access: Make sure you have the correct SSH private key to access the EC2 instance.
Next Steps
Customize the project to include additional Docker containers or other AWS resources.
Explore scaling EC2 instances or managing a multi-cloud setup with Terraform.
