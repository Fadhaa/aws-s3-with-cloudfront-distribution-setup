
# 🚀 Setup CloudFront with HTTPS for S3 Static Website

This guide explains how to deliver your S3-hosted static website securely using **CloudFront with HTTPS**.
Let's supposed that you created bucket name **statsolve-s3** and the Domain name is **mystatsolve.click**.

---

## 🧭 Why Use CloudFront?

- Adds **HTTPS support** (S3 static sites support only HTTP natively)
- Improves **performance** with global CDN caching
- Adds **security** options (signed URLs, WAF, rate limiting)
- Can connect to a **custom domain** (like `www.statsolve.com`)

---

## 🪜 Step-by-Step Setup

### 1️⃣ Prepare Your S3 Bucket
Make sure:
- **Static website hosting is enabled**
- Your files (e.g., `index.html`) are **publicly readable**
- You can access the **S3 Website Endpoint** (e.g., `http://YOUR-Bucket-NAME.s3-website-us-east-1.amazonaws.com`)
- In my example the **YOUR-Bucket-NAME** is **statsolve-s3**

---

### 2️⃣ Create a CloudFront Distribution

1. Go to **CloudFront** → Click **Create Distribution**
2. In the **Origin Settings**:
   - **Origin domain**: Choose your S3 **website endpoint** (⚠️ ends with `.amazonaws.com`, not the bucket name)
   - Set **Origin access** to `Public` (since website endpoint doesn't support OAC)
3. Under **Default cache behavior**:
   - Viewer Protocol Policy: `Redirect HTTP to HTTPS` ✅
   - Allowed HTTP methods: `GET, HEAD` (default)
4. Under **Settings**:
   - Provide a name (e.g., `statsolve-s3-cdn`)
   - (Optional) Add a **custom domain** and SSL (see below)
5. Click **Create Distribution**

---

### 3️⃣ (Optional) Connect a Custom Domain

To serve via `www.statsolve.com` or similar:

1. Go to **Route 53** (or your domain registrar)
2. Create a **CNAME** or **A record (alias)** pointing to your CloudFront domain name
3. Add the domain name in your CloudFront distribution settings
4. Use **ACM (AWS Certificate Manager)** to:
   - Request a certificate for `www.statsolve.com`
   - Validate via DNS
   - Attach the certificate in CloudFront → **SSL/TLS Settings**

---

### 4️⃣ Test the HTTPS Site

Once your distribution status = **Deployed**, open the CloudFront domain: 

https://www.statsolve.com

---

## 🔐 Security Notes

- CloudFront with **S3 website endpoint** cannot use Origin Access Control (OAC). If you want private buckets, you must use the **S3 REST endpoint** with OAC.
- Attach **AWS WAF** for DDoS protection or IP filtering (see next guide).

---

## 🖼 Example Architecture

![CloudFront + S3 + HTTPS](../assets/cloudfront-s3-https.png)

---

## ✅ Related Docs

- [AWS CloudFront Docs](https://docs.aws.amazon.com/cloudfront/)
- [S3 Website Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- [Use ACM for HTTPS](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request-public.html)




