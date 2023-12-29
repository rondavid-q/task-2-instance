# Terraform AWS EC2 Instance Setup

This Terraform script provisions an AWS EC2 instance with an associated key pair and stores the private key in an S3 bucket for secure key management.

## Prerequisites

Before running this Terraform script, make sure you have the following prerequisites:

- [Terraform](https://www.terraform.io/) installed on your machine.
- AWS credentials configured with the necessary permissions.

## Configuration

Update the `variables.tf` file to customize the EC2 instance and key pair configurations according to your requirements.

You can also change the security group ID and subnet mapping to have the Ec2 instance in the private subnet which was created while running the task-2-infra repo.


## Usage

1. Clone this repository:

   ```bash
   git clone https://github.com/rondavid-q/task-2-instance.git
   cd task-2-instance

   terraform init
   terraform plan
   terraform apply

2. This folder also has Jenkinsfile, which can be used with jenkins to automate the entire process along.

The jenkins file also has the flexibility to create and destroy resources.
