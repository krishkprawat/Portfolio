![Screenshot 2023-10-13 133850](https://github.com/krishkprawat/Portfolio/assets/47602022/80c7ae91-06d6-41d1-8f01-29e0b526ff45)

![Screenshot 2023-10-13 133909](https://github.com/krishkprawat/Portfolio/assets/47602022/51bfdd7b-36ed-4c8a-9f10-6d926a2cce63)



GitHub Actions and AWS provide a powerful combination for seamless deployments. In this guide, we'll walk you through setting up an automated workflow to deploy your portfolio website to an AWS S3 bucket.


Prerequisites
Before we get started, make sure you have the following in place:

1. A GitHub repository for your portfolio website.

2. AWS account with an S3 bucket where you want to deploy your website.

----Creating the GitHub Actions Workflow---
=================================================

First, let's set up the GitHub Actions workflow:

name: Portfolio Deployment

on:
  push:
    branches:
    - main


This workflow is triggered whenever there is a push event to the "main" branch of your repository. You can customize the branch as needed.

Job Configuration
====================
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
This workflow contains a single job named "build-and-deploy" that runs on an Ubuntu-based runner.

Deployment Steps
=======================

Now, let's define the steps for deployment:


COPY
codesteps:
- name: Checkout
  uses: actions/checkout@v1

This step checks out the code from your repository onto the runner. for this goto repository- settings - secrets and variables - add new repository secrets- add the acess key and id. (use same secret name AWS_ACCESS_KEY_ID [in this case]).



- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1
This step configures your AWS credentials using GitHub Secrets, ensuring that your AWS access keys are securely stored.


- name: Deploy static site to S3 bucket
  run: aws s3 sync . s3:<your s3 bucket name> --delete
This final step uses the AWS CLI to sync the contents of your repository with the specified S3 bucket. It also deletes any objects in the bucket that aren't in your repository.





To see your website (static website hosting - goto s3 bucket- description - static website hosting - enable - add file name (index.html) and save.

if in case there is permission error then use a bucket policy file and add this to s3 bucket policy section - file is attached in the github repo under readme file.



===============================================
use this s3 permission file into s3 bucket ---=
==================================================
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:*" #we can give Lsit object also but now i am giving all permission
            ],
            "Resource": "arn:aws:s3:::kp-portfolio/*"
        }
    ]
}
