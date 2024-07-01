# FitLife Weekly - Serverless Email Marketing Application

Project Overview
FitLife Weekly is a serverless email marketing application designed to send health and fitness newsletters to subscribers. The application leverages AWS services including Simple Email Service (SES), EventBridge, S3, Lambda, and Identity & Access Management (IAM) to create a scalable and cost-effective solution for email marketing campaigns.

AWS Services Utilized
Simple Email Service (SES): For sending emails.
EventBridge: For scheduling and triggering Lambda functions.
S3: For storing email templates and contact lists.
Lambda: For executing email sending logic.
Identity & Access Management (IAM): For managing permissions and roles.
Project Steps

1. Setting Up S3
Create an S3 bucket to store email templates and contact lists.
Upload the HTML email template and CSV file with contact information to the S3 bucket.
2. HTML Email Template
Use an HTML email template for newsletters with placeholders for personalization.
3. Setting Up Amazon SES
Verify email addresses and domain in Amazon SES.
Configure SES to send emails using verified email addresses.
4. Moving SES from Sandbox to Production
Request production access for SES to lift sandbox restrictions.
Validate recipient email addresses.
5. Creating the Lambda Function
Create a Python Lambda function to handle email logic.
6. Configuring IAM Permissions
Create an IAM policy with permissions for SES and S3.
Attach the policy to the Lambda execution role.
7. Testing the Lambda Function
Configure a test event for the Lambda function.
Run the Lambda function to ensure it retrieves files from S3 and sends emails via SES.
8. Scheduling Email Sends with EventBridge
Create a schedule using EventBridge to trigger the Lambda function.
Conclusion
FitLife Weekly demonstrates how to use AWS services to build a serverless email marketing application. By leveraging SES, S3, Lambda, and EventBridge, the application provides a scalable, cost-effective solution for sending personalized newsletters to subscribers.
