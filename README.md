# 🚀 Static Website Hosting with Amazon S3 + CloudFront + Origin Access Control (OAC)

This project demonstrates how I hosted my **static website** securely and globally using **Amazon S3**, **CloudFront**, and **Origin Access Control (OAC)**.
---

## 📌 Project Overview

- **Type:** Static Website Hosting
- **Cloud Provider:** AWS
- **Services Used:**
  - Amazon S3 – Object storage for static files
  - Amazon CloudFront – Global CDN for low-latency delivery
  - Origin Access Control (OAC) – Restricts direct S3 access
  - AWS IAM – Access management
- **Goal:** Deliver a fast, secure, and globally available static site without making the S3 bucket public.

---

## 🏗 Architecture

[User Browser] ⇄ [CloudFront CDN] ⇄ [OAC] ⇄ [Amazon S3 Bucket]




---

## ⚙️ Implementation Steps

### **Step 1 – Create S3 Bucket**
1. Open **AWS S3 Console** → **Create bucket**.
2. Choose a globally unique bucket name (e.g., `myportfolio-bucket`).
3. Select the closest AWS region.
4. **Block all public access** → **ON**.
5. Upload your website files (`index.html`, `style.css`, `script.js`, images).
6. (Optional) Enable **Static Website Hosting** for testing.

📸 Screenshot:  
![S3 Bucket Setup](screenshots/s3-bucket-setup.png)

---

### **Step 2 – Create CloudFront Distribution**
1. Go to **AWS CloudFront** → **Create Distribution**.
2. **Origin domain**: Select your S3 bucket.
3. **Origin access**: Create a new **Origin Access Control (OAC)**.
4. Viewer Protocol Policy: Redirect HTTP → HTTPS.
5. Allowed HTTP Methods: GET, HEAD.
6. Enable **Object Compression** for faster delivery.
7. (Optional) Add a **Custom Domain** and link an ACM SSL certificate.

📸 Screenshot:  
![CloudFront Distribution](screenshots/cloudfront-distribution.png)

---

### **Step 3 – Configure S3 Bucket Policy for OAC**
- Go to your bucket → **Permissions** → **Bucket Policy**.
- Paste a policy like this (replace with your actual details):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontAccess",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::myportfolio-bucket/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::123456789012:distribution/E1ABCXYZ123"
        }
      }
    }
  ]
}

```

Step 4 – Invalidate CloudFront Cache
If you update your site and changes don’t appear:

CloudFront → Invalidations → Create Invalidation.

Path: /* → Create.

Step 5 – Test Your Website
CloudFront URL:

https://d3asgsndmtcp1g.cloudfront.net/index.html


✅ Benefits of This Setup
Secure → Private S3 bucket, no public access.

Fast → CloudFront CDN delivers content globally.

Scalable → Handles large traffic automatically.

Cost-Effective → Pay only for what you use.

⚠️ Challenges Faced
AccessDenied Error when OAC policy was missing.

CloudFront serving old cached errors — fixed with invalidation.

Ensuring case-sensitive file names in S3 matched site references.


🙌 Author
Ben 🐼
Cloud Security & AWS Enthusiast
  
