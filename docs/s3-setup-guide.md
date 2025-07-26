# 📦 AWS S3 Setup Guide (Console-Based)-Updated 26 July 2025

This guide walks you through setting up an Amazon S3 bucket directly from the AWS Management Console — without using the CLI or scripts.

---

## ✅ Prerequisites

- An active [AWS account](https://aws.amazon.com/)
- IAM user or role with S3 permissions (`AmazonS3FullAccess` or custom permissions)

---

## 🪜 Step-by-Step Instructions

### 1️⃣ Create an S3 Bucket

1. Log in to the [AWS Console](https://console.aws.amazon.com/).
2. Navigate to **S3** .
3. Click **Create bucket**.
4. Fill in:
   - **AWS Region** (choose closest to your target audience)
   - **Bucket type** (choose General purpose)
   - **Bucket name** (must be globally unique). In this example, let's choose **statsolve.click**
5. If you have previous bucket, you can Copy settings from existing bucket - optional. otherwise continue to set new one
   
6. Leave "Object Ownership" as **ACLs disabled** (recommended).
7. Under **Block Public Access**, deselect **Block all public access** then tick "I acknowledge that the current settings might result in this bucket and the objects within becoming public" to enable static website hosting.
8. Under **Bucket Versioning** keep it disabled for now. leave all other settings as default.
9. Click **Create bucket**.

---

### 2️⃣ Go to Amazon S3> Buckets and click on your bucket name. In this example **statsolve.click**

1. Go to the **Properties** tab.
2. Under **Static website hosting** click **Edit**
   - under **Static website hosting** select **Enable**
   - under **Index document** enter **index.html**
   - under **Error document - optional** enter **error.html**
   - Leave all outher settings as default
3. click **Save changes**



### 3️⃣ Add Permissions

1. Under **Bucket policy** click **Edit**
   - Copy the following policy and paste it in the box
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::statsolve.click/*"
        }
    ]
}

```

2. click on **Save Changes**


---

### 4️⃣ (Optional) Enable Static Website Hosting

1. Go to the **Properties** tab.
2. Scroll to **Static website hosting**.
3. Click **Edit**.
4. Enable it and set:
   - **Index document**: `index.html`
   - (Optional) **Error document**: `error.html`
5. Save changes.

---

### 5️⃣ (Optional) Modify Public Access Settings

> ⚠️ Only do this if you are hosting a public website.

1. From the **Permissions** tab:
2. Scroll to **Block public access**.
3. Click **Edit**.
4. Uncheck the blocking options as needed.
5. Confirm by typing **confirm** and save.

---

### 6️⃣ (Optional) Attach a Bucket Policy

If making the bucket public, attach a public-read policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
