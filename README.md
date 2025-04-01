# 🖼️ Image Label Generator with AWS Rekognition

This project uses **Amazon S3**, **Lambda**, and **Rekognition** to detect labels in uploaded images. It’s a simple, serverless application that takes an image filename from S3 and returns objects detected in the image with confidence scores.

🔗 Tech Stack: `AWS Lambda`, `S3`, `Amazon Rekognition`, `Python`

---

## 🧠 BUILD Summary

### 🔹 B – Background
This project was designed as a practical way to explore AWS’s machine learning capabilities using Rekognition. The goal was to analyze images using a fully serverless setup and return meaningful labels and confidence scores — all without needing a dedicated server or frontend.

---

### 🔹 U – Understanding
The application flow is:
1. An image is uploaded to an S3 bucket
2. A Lambda function is triggered manually (or later by API)
3. The image is passed to Amazon Rekognition
4. Rekognition returns label data (e.g., “Dog”, “Tree”, “Person”)
5. The function returns a JSON response with the detected labels and confidence scores

Input Example:
```json
{
  "bucket": "your-bucket-name",
  "photo": "dog.jpg"
}

---

### 🔹 I – Implementation
Language: Python 3.12
Services Used:
Amazon S3 for image storage
AWS Lambda for compute logic
Amazon Rekognition for image analysis
IAM for permission control

response = client.detect_labels(
    Image={'S3Object': {'Bucket': bucket, 'Name': photo}},
    MaxLabels=10,
    MinConfidence=75
)
IAM Role permissions:
AmazonS3ReadOnlyAccess
AmazonRekognitionFullAccess

---

### 🔹 L – Lessons Learned
Rekognition is incredibly powerful even with minimal setup.

Lambda permissions can be tricky — ensuring the right IAM roles and S3 bucket policy was key.

File naming in S3 matters (no commas or spaces!).

CloudWatch Logs helped debug and verify behavior.

---

### 🔹 D – Deliverables
✅ Working Lambda function
✅ S3 integration
✅ Rekognition output tested
🕒 Time to build: ~1.5 hours
🚀 Next steps: Add API Gateway or web frontend






