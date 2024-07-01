FitLife Weekly - Serverless Email Marketing Application
Project Overview
FitLife Weekly is a serverless email marketing application designed to send health and fitness newsletters to subscribers. The application leverages AWS services including Simple Email Service (SES), EventBridge, S3, Lambda, and Identity & Access Management (IAM) to create a scalable and cost-effective solution for email marketing campaigns.

AWS Services Utilized
Simple Email Service (SES): For sending emails.
EventBridge: For scheduling and triggering Lambda functions.
S3: For storing email templates and contact lists.
Lambda: For executing email sending logic.
Identity & Access Management (IAM): For managing permissions and roles.
Prerequisites
To follow along with this project, you will need:

Email addresses (to send to) that you can validate.
An email address (to send from) that you can validate.
Basic knowledge of AWS.
A text editor for writing HTML code.
Project Steps
1. Setting Up S3
Create an S3 bucket to store your email templates and contacts.

Go to the AWS Management Console.
Navigate to S3.
Click on "Create bucket".
Enter a unique bucket name and select a region.
Click "Create bucket".
Upload the HTML email template and the CSV file with contact information to the S3 bucket.

Go to your S3 bucket.
Click "Upload".
Select your HTML email template file and the CSV file.
Click "Upload".
