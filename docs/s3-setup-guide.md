# 📦 AWS S3 Setup Guide (Console-Based)

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
   - **Bucket name** (must be globally unique)
   - **AWS Region** (choose closest to your target audience)
5. Leave "Object Ownership" as **ACLs disabled** (recommended).
6. Under **Block Public Access**, keep all boxes checked unless hosting a static website.
7. Click **Create bucket**.

---

### 2️⃣ Enable Bucket Versioning (for backups & recovery)

1. Go to your new bucket.
2. Click the **Properties** tab.
3. Scroll down to **Bucket Versioning**.
4. Click **Edit**, select **Enable**, and click **Save changes**.

---

### 3️⃣ Add Lifecycle Rules (e.g., archive to Glacier)

1. Go to the **Management** tab.
2. Click **Create lifecycle rule**.
3. Name your rule (e.g., `ArchiveAfter30Days`).
4. Choose:
   - **This rule applies to all objects in the bucket**
   - Or limit by prefix/tag
5. Under **Lifecycle rule actions** choose
   - Transition current versions of objects between storage classes
5. Under **Transitions**, add:
   - Move current versions to **Glacier Instant Retrieval** after **30 days**
6. Click **Create rule**.

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
