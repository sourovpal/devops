# Terraform Setup

Terraform হলো একটি Infrastructure as Code (IaC) টুল।\
কোড লিখে সার্ভার, নেটওয়ার্ক, ডাটাবেস, লোড ব্যালান্সার ইত্যাদি তৈরি ও ম্যানেজ করা।

### Install Terraform
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
sudo apt-add-repository "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
```
Verify install: `sudo terraform -v`

### Terraform Project File Structure

project/\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── main.tf      # main configuration\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── variables.tf # variables declaration\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── outputs.tf   # outputs declaration\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── terraform.tfvars # variable values

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


