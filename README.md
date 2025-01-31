# Automating NCAA Game Highlights with AWS & Docker

## A Scalable and Automated Solution (Part 1: Storage & Playback)

### Introduction
Content creators and media companies face a constant challenge: efficiently managing and distributing game highlights. This project tackles this by building a **scalable and automated solution** powered by Docker and AWS.

### Problem
- **85% of sports content creators** struggle with organizing and processing video libraries.
- Scattered highlights across platforms require time-consuming manual effort for sharing.

### Solution
The **Game Highlight Processor** automates:
- Game highlight retrieval
- Video storage in AWS S3 (scalable, secure, cost-effective)
- (Media processing - details in Part 2)

### Benefits
- **Content Creators:** Save time, easily repurpose content across platforms.
- **Media Companies:** Streamline workflows, ensure consistent quality.
- **Sports Fans:** Faster access to high-quality highlights, seamless viewing.

## Prerequisites
- **AWS Account:** Create an AWS account (prior AWS service familiarity helpful). Use AWS CloudShell (AWS CLI pre-installed). Obtain AWS Access Key and Secret Key from IAM.
- **RapidAPI Account:** Create a free RapidAPI.com account and get an API key for the Sports Highlights API (NCAA highlights).
- **Python 3.x:** Ensure Python is installed locally for testing and debugging scripts.
- **Docker:** Verify installation with `docker --version`.

## Step-by-Step Implementation

### 1. Download the Project Files
Clone the repository using Git:
```bash
git clone https://github.com/danielhensha2/game-highlight-processor.git
cd game-highlight-processor/src
```

### 2. Configure AWS and Add API Key
#### a. Configure AWS CLI:
```bash
aws configure
```
Replace placeholders with your actual credentials and region.

#### b. Add API Key to Secrets Manager:
```bash
aws secretsmanager create-secret \
  --name my-api-key \
  --description "API key for accessing the Sport Highlights API" \
  --secret-string '{"api_key":"YOUR_ACTUAL_API_KEY"}' \
  --region us-east-1
```

### 3. Create an IAM Role for Secure Access
1. In the AWS Management Console, search for "IAM".
2. Create a role with "S3" use case.
3. Attach permissions:
   - AmazonS3FullAccess
   - MediaConvertFullAccess
   - AmazonEC2ContainerRegistryFullAccess
4. Name the role (e.g., "HighlightProcessorRole").
5. Optionally, update the trust policy.

### 4. Configure Environment Variables
Create a `.env` file in the `src` directory and add your environment variables:
```bash
RapidAPI_KEY=...  # Your RapidAPI key
AWS_ACCESS_KEY_ID=...  # Your AWS access key ID
AWS_SECRET_ACCESS_KEY=...  # Your AWS secret access key
S3_BUCKET_NAME=...  # Your S3 bucket name
MEDIACONVERT_ENDPOINT=...  # AWS MediaConvert endpoint (use `aws mediaconvert describe-endpoints`)
MEDIACONVERT_ROLE_ARN=...  # Your IAM role ARN
```

### 5. Secure Your Environment Variables
Restrict access to `.env` with:
```bash
chmod 600 .env
```

### 6. Build and Run the Dockerized Application
#### a. Build the Docker Image:
```bash
docker build -t highlight-processor .
```
#### b. Run the Container:
```bash
docker run --env-file .env highlight-processor
```
This executes scripts (`fetch.py`, `process_one_video.py`, `mediaconvert_process.py`) for:
- Automatic game highlight retrieval
- Storage in your S3 bucket
- (Potential media processing - details in Part 2)

### 7. Verify Successful Execution
Check your S3 bucket for uploaded files (e.g., `first_video.mp4` in the `videos` folder).

## Conclusion (Part 1)
We've automated fetching and storing NCAA game highlights using Docker and AWS S3, laying the foundation for a robust and scalable sports highlight processing pipeline.

### Stay tuned for Part 2, which will delve into:
- **AWS MediaConvert Integration:** Video transcoding and optimization.
- **Terraform Provisioning:** Infrastructure provisioning (S3 buckets, IAM roles) for automated setup.
- **Enhanced Scalability:** Exploring ways to handle larger data volumes and improve performance.

---

For further details, feel free to explore the repository or contribute to the project!

