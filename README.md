# Portfolio




use this s3 permission file into s3 bucket ---
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:*"
            ],
            "Resource": "arn:aws:s3:::kp-portfolio/*"
        }
    ]
}