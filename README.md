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
Upload the HTML email template and the CSV file with contact information to the S3 bucket.
2. HTML Email Template
We use the following HTML email template for our newsletters:

html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FitLife Weekly - Health and Nutrition for Fitness Enthusiasts</title>
<style>
  /* Add your styles here */
</style>
</head>
<body>
<div class="container">
  <!-- Your content here -->
</div>
</body>
</html>
3. Setting Up Amazon SES
Verify your email address and domain in Amazon SES.
Configure SES to send emails using your verified email address.
4. Moving SES from Sandbox to Production
Request production access for SES to lift sandbox restrictions.
Validate email addresses you plan to send emails to.
5. Creating the Lambda Function
Create a new Python Lambda function for handling email logic.
Lambda function code:
python
Copy code
import boto3
import csv

# Initialize the boto3 client
s3_client = boto3.client('s3')
ses_client = boto3.client('ses')

def lambda_handler(event, context):
    # Specify the S3 bucket name
    bucket_name = 'sp-email-marketing-project'

    try:
        # Retrieve the CSV file from S3
        csv_file = s3_client.get_object(Bucket=bucket_name, Key='contacts.csv')
        lines = csv_file['Body'].read().decode('utf-8').splitlines()
        
        # Retrieve the HTML email template from S3
        email_template = s3_client.get_object(Bucket=bucket_name, Key='email_template.html')
        email_html = email_template['Body'].read().decode('utf-8')
        
        # Parse the CSV file
        contacts = csv.DictReader(lines)
        
        for contact in contacts:
            # Replace placeholders in the email template with contact information
            personalized_email = email_html.replace('{{FirstName}}', contact['FirstName'])
            
            # Send the email using SES
            response = ses_client.send_email(
                Source='praneethsai8184@gmail.com',  # Replace with your verified "From" address
                Destination={'ToAddresses': [contact['Email']]},
                Message={
                    'Subject': {'Data': 'Your Weekly FitLife Mail!', 'Charset': 'UTF-8'},
                    'Body': {'Html': {'Data': personalized_email, 'Charset': 'UTF-8'}}
                }
            )
            print(f"Email sent to {contact['Email']}: Response {response}")
    except Exception as e:
        print(f"An error occurred: {e}")
6. Configuring IAM Permissions
Create a policy with permissions for SES and S3.
Attach the policy to the Lambda execution role.
json
Copy code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::sp-email-marketing-project/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ses:SendEmail",
                "ses:SendRawEmail"
            ],
            "Resource": "*"
        }
    ]
}
7. Testing the Lambda Function
Configure a test event for the Lambda function.
json
Copy code
{
  "comment": "Generic test event for scheduled Lambda execution. The function does not use this event data.",
  "test": true
}
Run the Lambda function to ensure it can retrieve files from S3 and send emails via SES.
8. Scheduling Email Sends with EventBridge
Create a new schedule using EventBridge to trigger the Lambda function.
Test the EventBridge schedule to confirm emails are sent on time.

Conclusion
FitLife Weekly demonstrates the power of serverless architecture in building scalable and efficient email marketing solutions. By leveraging AWS services, we can automate the process of sending personalized emails, making it easier to manage and maintain large-scale email campaigns.

