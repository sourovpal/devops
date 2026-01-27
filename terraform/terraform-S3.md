# AWS S3 Bucket

### 📌 S3 Bucket **Public Read Enable**
**S3 → Your Bucket → Permissions**

- Block public access → Edit
- ❌ সব checkbox আনচেক করুন
- Save changes

### 📌 Add Bucket Policy
**Permissions → Bucket policy → Edit**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```
