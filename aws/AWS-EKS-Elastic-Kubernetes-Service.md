# Elastic Kubernetes Service

### Show cluster list
```bash
aws eks list-clusters --region ap-southeast-1
```

### Show all cluster list any region
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

## Create a new cluster

### Create Role
```bash
aws iam create-role \
  --role-name EKSClusterRole \
  --assume-role-policy-document file://eks-trust.json
```
* Role বানানোর সময় তুমি যেকোনো meaningful নাম দিতে পারবে

### Policy attach
```bash
aws iam attach-role-policy \
  --role-name EKSClusterRole \
  --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy
```
* যদি তুমি নিজে policy বানাও নাম পুরোপুরি তোমার ইচ্ছেমতো
* AWS Managed Policy হলে যদি তুমি AWS-এর built-in policy attach করো ❌ নাম change করা যাবে না
