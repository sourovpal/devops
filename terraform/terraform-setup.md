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
### Create VPC

```tf
provider "aws" {
  region = "ap-southeast-1"
}

resource "aws_vpc" "my_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "MyVPC"
  }
}
```

### Create Public Subnet
```tf
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.my_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true  # EC2 automatically public IP assign করবে
  availability_zone       = "ap-southeast-1a"

  tags = {
    Name = "PublicSubnet"
  }
}
```


### Create Internet Gateway
```tf
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.my_vpc.id

  tags = {
    Name = "MyIGW"
  }
}
```

### Create Route Table
```tf
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.my_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "PublicRouteTable"
  }
}
```

### Associate Route Table with Subnet

```tf
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}
```

### Security Group for SSH and HTTP
```tf
resource "aws_security_group" "my_sg" {
  name        = "my_sg"
  description = "Allow SSH and HTTP"
  vpc_id      = aws_vpc.my_vpc.id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "MySecurityGroup"
  }
}
```
### Create Instance 

```tf
resource "aws_instance" "my_instance" {
  ami                    = "ami-08d59269edddde222"  # region specific
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.public_subnet.id
  vpc_security_group_ids = [aws_security_group.my_sg.id]
  key_name               = "workrobot"

  associate_public_ip_address = true

  tags = {
    Name = "MyTerraformInstance"
  }
}
```

### `main.tf`
```tf
provider "aws" {
  region = "ap-southeast-1"
}

# 1. Create VPC
resource "aws_vpc" "my_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "MyVPC"
  }
}

# 2. Create Public Subnet
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.my_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true  # EC2 automatically public IP assign করবে
  availability_zone       = "ap-southeast-1a"

  tags = {
    Name = "PublicSubnet"
  }
}

# 3. Create Internet Gateway
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.my_vpc.id

  tags = {
    Name = "MyIGW"
  }
}

# 4. Create Route Table
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.my_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "PublicRouteTable"
  }
}

# 5. Associate Route Table with Subnet
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}

# 6. Security Group for SSH and HTTP
resource "aws_security_group" "my_sg" {
  name        = "my_sg"
  description = "Allow SSH and HTTP"
  vpc_id      = aws_vpc.my_vpc.id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "MySecurityGroup"
  }
}

# 7. EC2 Instance
resource "aws_instance" "my_instance" {
  ami                    = "ami-08d59269edddde222"  # region specific
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.public_subnet.id
  vpc_security_group_ids = [aws_security_group.my_sg.id]
  key_name               = "workrobot"

  associate_public_ip_address = true

  tags = {
    Name = "MyTerraformInstance"
  }
}

```
### Private Subnet + Create NAT Gateway + Route Table for Private Subnet
```tf
# 3. Create Private Subnet
resource "aws_subnet" "private_subnet" {
  vpc_id            = aws_vpc.my_vpc.id
  cidr_block        = "10.0.2.0/24"
  availability_zone = "ap-southeast-1a"
  map_public_ip_on_launch = false  # Private subnet, তাই false

  tags = {
    Name = "PrivateSubnet"
  }
}

# Elastic IP for NAT
resource "aws_eip" "nat_eip" {
  vpc = true
}

# NAT Gateway in Public Subnet
resource "aws_nat_gateway" "nat" {
  allocation_id = aws_eip.nat_eip.id
  subnet_id     = aws_subnet.public_subnet.id

  tags = {
    Name = "MyNATGateway"
  }
}


# Private Route Table
resource "aws_route_table" "private_rt" {
  vpc_id = aws_vpc.my_vpc.id

  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.nat.id
  }

  tags = {
    Name = "PrivateRouteTable"
  }
}

# Associate Private Subnet with Private Route Table
resource "aws_route_table_association" "private_assoc" {
  subnet_id      = aws_subnet.private_subnet.id
  route_table_id = aws_route_table.private_rt.id
}

```

