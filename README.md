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

**Test Input Example:**
```json
{
  "bucket": "your-bucket-name",
  "photo": "dog.jpg"
}

### 🔹 I – Implementation

- **Language:** Python 3.12  
- **AWS Services Used:**
  - 🪣 Amazon S3 – to store uploaded images  
  - ⚙️ AWS Lambda – to run the detection logic  
  - 👁️ Amazon Rekognition – to identify objects  
  - 🔐 IAM – to grant permissions

**Key IAM Permissions:**

- `AmazonS3ReadOnlyAccess`  
- `AmazonRekognitionFullAccess`

**Lambda Function Core Logic:**

```python
import boto3
import json

def lambda_handler(event, context):
    bucket = event['bucket']
    photo = event['photo']

    client = boto3.client('rekognition')

    response = client.detect_labels(
        Image={
            'S3Object': {
                'Bucket': bucket,
                'Name': photo
            }
        },
        MaxLabels=10,
        MinConfidence=75
    )

    labels = []
    for label in response['Labels']:
        labels.append({
            'Name': label['Name'],
            'Confidence': round(label['Confidence'], 2)
        })

    return {
        'statusCode': 200,
        'body': json.dumps(labels)
    }
