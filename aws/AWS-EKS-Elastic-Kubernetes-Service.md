# ☸️ Elastic Kubernetes Service

### 🧩 Show cluster list
```bash
aws eks list-clusters --region ap-south-1
```

### 🧩 Show all cluster list any region
```bash
# Format table
for region in $(aws ec2 describe-regions --query "Regions[].RegionName" --output text); do
  echo "# Region: $region"
  aws eks list-clusters --region $region --output table
done

# Format Json
for region in $(aws ec2 describe-regions --query "Regions[].RegionName" --output text); do
  echo "# Region: $region"
  aws eks list-clusters --region $region
done
```

## 🔀 Create a new cluster

### 🧩 Create Role
`eks-trust.json`
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "eks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
```bash
aws iam create-role \
  --role-name EKSClusterRole \
  --assume-role-policy-document file://eks-trust.json
```
* Role বানানোর সময় তুমি যেকোনো meaningful নাম দিতে পারবে

### 🧩 Policy attach
```bash
aws iam attach-role-policy \
  --role-name EKSClusterRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy
```


* যদি তুমি নিজে policy বানাও নাম পুরোপুরি তোমার ইচ্ছেমতো
* AWS Managed Policy হলে যদি তুমি AWS-এর built-in policy attach করো ❌ নাম change করা যাবে না

### 🧩 EKS Cluster Create
```bash
aws eks create-cluster \
  --name my-eks-cluster \
  --region ap-south-1 \
  --kubernetes-version 1.29 \
  --role-arn arn:aws:iam::<ACCOUNT_ID>:role/EKSClusterRole \
  --resources-vpc-config subnetIds=subnet-aaa,subnet-bbb,endpointPublicAccess=true,endpointPrivateAccess=false
```
* 📌 এখানে:
  - subnet-aaa, subnet-bbb → তোমার VPC এর subnet ID
  - <ACCOUNT_ID> → AWS Account ID
  - EKS API internet থেকে access করা যাবে (endpointPublicAccess=true,endpointPrivateAccess=false)

### 🧩 Describe Cluster
```bash
aws eks describe-cluster --name Html-Project-EKS --region ap-south-1
```

### 🧩 kubeconfig Update
```bash
aws eks update-kubeconfig \
  --name my-eks-cluster \
  --region ap-south-1

# Get all nodes

kubectl get nodes
```

## 🗄️ Node Group

### 🧩 Create Role
`trust-policy.json`
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
```bash
aws iam create-role \
    --role-name EKSNodeRole \
    --assume-role-policy-document file://trust-policy.json
```

### 🧩 Attach Role Policy
```bash
aws iam attach-role-policy \
    --role-name EKSNodeRole \
    --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy

aws iam attach-role-policy \
    --role-name EKSNodeRole \
    --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy

aws iam attach-role-policy \
    --role-name EKSNodeRole \
    --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly
```
* 📌 Node role এ লাগবে:
  - AmazonEKSWorkerNodePolicy
  - AmazonEKS_CNI_Policy
  - AmazonEC2ContainerRegistryReadOnly

### 🧩 Create Node Group for EKS cluster

```bash
aws eks create-nodegroup \
  --cluster-name my-eks-cluster \
  --nodegroup-name my-node-group \
  --subnets subnet-aaa subnet-bbb \
  --instance-types t3.medium \
  --scaling-config minSize=1,maxSize=3,desiredSize=2 \
  --node-role arn:aws:iam::<ACCOUNT_ID>:role/EKSNodeRole \
  --region ap-south-1
```

### 🧩 Check Nodes

```bash
kubectl get nodes
```
