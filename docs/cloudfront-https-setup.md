
# 🚀 Setup CloudFront with HTTPS for S3 Static Website (Updated 26 July 2025).

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
- In my example the **YOUR-Bucket-NAME** is **statsolve.click**
- For setting S3 bucket, follow this guidance (https://github.com/Fadhaa/aws-s3-with-cloudfront-distribution-setup/blob/main/docs/s3-setup-guide.md)


---
####  You also need to create TLS certificate
1. Go to **AWS Certificate Manager (ACM)**
2. Click **Request**
3. Under **Certificate type**, select **Request a public certificate** and then click **Next**
4. In **Fully qualified domain name**, enter your domain name. In my case, it's **mystatsolve.click**
5. Leave all other settings as default and click **Request**
6. Go to **Certificates** and click on the certificate that has your domain name.
7. In the **Domains**, click **Create records in Route 53** and then click **Create records** to add CNAME of your certificate in your Route 53.
8. Wait untill the validation completing and proceed to setup your cloudfront distribution



---

### 2️⃣ Create a CloudFront Distribution

1. Go to **CloudFront** → Click **Create Distribution**
2. In the **Distribution Name** choose **statsolvedist** or any name. And then click **Next**
3. Under **Origin type** choose **Amazon S3**
4. Under **Origin** for **S3 Origin**, enter **statsolve.click.s3-website-us-east-1.amazonaws.com**.
5. Leave the other settings as default, and click **Next**
6. In **Enable security**, select **Enable security protections**
7. Skip everything else and click **Create distribution**
8. After creating the distribution **statsolvedist**
9. Under General (settings), click **Edit**
10. Under **Alternate domain name (CNAME) - optional**, click **Add item** to enter your **domain name**. In this example my domain is **mystatsolve.click**.
11. In **Custom SSL certificate - optional**, choose the certificate you created above.
12. In **Default root object - optional** enter **index.html**
13. At the bottom, click **Save changes**.
14. In **General** tap of your distribution and under **Settings**, click **Route domains to CloudFront**
15. You need to **Set up DNS routing**. just click on **Set up routing automatically** to add two records in your route 53.

---

### 3️⃣ Test the HTTPS Site

Once your distribution status = **Deployed**, open your browser and check out your website. in my case, my website is https://mystatsolve.com : 




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




