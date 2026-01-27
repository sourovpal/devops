# Terraform Setup

Terraform হলো একটি Infrastructure as Code (IaC) টুল।\
কোড লিখে সার্ভার, নেটওয়ার্ক, ডাটাবেস, লোড ব্যালান্সার ইত্যাদি তৈরি ও ম্যানেজ করা।

### Install Terraform
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
sudo apt-add-repository "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform

# --------------------------------- OR ------------------------------------
sudo apt update
sudo apt install -y wget unzip

wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
unzip terraform_1.6.6_linux_amd64.zip
sudo mv terraform /usr/local/bin/

terraform -v

```
Verify install: `sudo terraform -v`

### Terraform Project File Structure
```text
project/\
├── main.tf      # main configuration
├── variables.tf # variables declaration
├── outputs.tf   # outputs declaration
└── terraform.tfvars # variable values
```
### Configure Terraform Provider
` main.tf`
```bash
terraform {        # ব্লকটি মূলত Terraform configuration এর global settings define করে।
  required_providers {
    aws = {
      source  = "hashicorp/aws"        # Terraform কে বলে কোন provider registry থেকে AWS provider আনতে হবে।
      version = "~> 5.0"
    }
  }
  required_version = ">= 1.5.0"
}

provider "aws" {           # এটা বলে Terraform কে তুমি কোন cloud provider ব্যবহার করবে। এখানে AWS। Google Cloud | Azure
  region     = "us-east-1"    # AWS এর কোন region এ resources তৈরি হবে। (যেমন EC2, S3, RDS ইত্যাদি)
  access_key = "YOUR_AWS_ACCESS_KEY"
  secret_key = "YOUR_AWS_SECRET_KEY"   # access_key এবং secret_key → AWS account authenticate করার জন্য credentials।
}
```

Terraform Commands
```bash
terraform init     # Terraform initialize
terraform plan     # কী হবে দেখাবে
terraform apply    # Resource তৈরি করবে
terraform destroy  # সব মুছে ফেলবে
```

### 📁 Professional Folder Structure

```text
terraform-project/
│
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   ├── ec2/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   └── rds/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
├── envs/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── terraform.tfvars
│   │
│   ├── staging/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   └── terraform.tfvars
│   │
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       └── terraform.tfvars
│
├── providers.tf
├── backend.tf
├── versions.tf
└── README.md
```

### Terraform Create AWS Instance

`main.tf`

```tf
# Provider নির্ধারণ
provider "aws" {
  region = "us-east-1"  # আপনার প্রয়োজনীয় region
}

# EC2 instance তৈরি
resource "aws_instance" "my_instance" {
  ami           = "ami-0c55b159cbfafe1f0"  # Ubuntu 22.04 AMI (region-specific)
  instance_type = "t2.micro"

  # Optional: Key pair for SSH access
  key_name = "my-key"  # আগেই AWS এ key pair তৈরি করে নিতে হবে

  # Optional: Subnet and Security group
  subnet_id              = "subnet-010486826dce7f158" # public subnet
  vpc_security_group_ids = ["sg-0123456789abcdef0"]  # আপনার security group ID

  associate_public_ip_address = true # public IP পেতে হলে অবশ্যই দিতে হব

  tags = {
    Name = "MyTerraformInstance"
  }
}
```
📌 subnet_id অবশ্যই security group এর VPC Same হতে হবে।

