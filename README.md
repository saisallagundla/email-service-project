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



2. HTML Email Template
Use the following HTML email template for your newsletters:

html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FitLife Weekly - Health and Nutrition for Fitness Enthusiasts</title>
<style>
  body {
    font-family: 'Arial', sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f5f5f5;
    color: #333;
  }
  .container {
    max-width: 800px;
    margin: auto;
    background: #ffffff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }
  .header {
    background-color: #28a745;
    color: white;
    padding: 15px 20px;
    border-radius: 8px 8px 0 0;
    text-align: center;
  }
  .header h1 {
    margin: 0;
    font-size: 28px;
  }
  .content h2 {
    color: #28a745;
    margin-top: 0;
  }
  .content p {
    font-size: 18px;
    line-height: 1.6;
  }
  .section {
    background-color: #e9f7ef;
    border-left: 5px solid #28a745;
    padding: 15px;
    margin: 20px 0;
  }
  .footer {
    text-align: center;
    font-size: 16px;
    color: #777;
    margin-top: 30px;
  }
  .footer a {
    color: #28a745;
    text-decoration: none;
  }
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <h1>FitLife Weekly</h1>
  </div>
  <div class="content">
    <p>Hello {{FirstName}},</p>
    <p>Welcome to <strong>FitLife Weekly</strong>,your go-to newsletter for the latest tips, trends, and insights in health, nutrition, and fitness. Whether you're a seasoned lifter or just starting your fitness journey, we've got something for everyone.</p>
    <!-- Content continues -->
  </div>
  <div class="footer">
    <p>If you no longer wish to receive these emails, you can <a href="#">unsubscribe here</a>.</p>
  </div>
</div>
</body>
</html>




3. Setting Up Amazon SES
Verify your email address and domain in Amazon SES.

Go to the AWS Management Console.
Navigate to SES.
Click "Verify a New Email Address".
Enter your email address and click "Verify This Email Address".
Follow the instructions in the verification email sent to your address.
Configure SES to send emails using your verified email address.
In SES, go to "Email Addresses" and ensure your email address is verified.



4. Moving SES from Sandbox to Production
Request production access for SES to lift sandbox restrictions.

Go to the AWS Management Console.
Navigate to SES.
Click "Sending Statistics".
Click "Request a Sending Limit Increase".
Fill out the request form and submit it.
Validate email addresses you plan to send emails to.
Ensure all recipient email addresses are verified in SES.


5. Creating the Lambda Function
Create a new Python Lambda function for handling email logic.

Go to the AWS Management Console.
Navigate to Lambda.
Click "Create function".
Select "Author from scratch".
Enter a function name and choose Python 3.8 as the runtime.
Click "Create function".
Lambda function code:

python

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
Go to the AWS Management Console.
Navigate to IAM.
Click "Policies" and then "Create policy".
Add the following policy JSON:
json



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



Attach the policy to the Lambda execution role.
Go to the AWS Management Console.
Navigate to Lambda.
Select your function and click on the "Configuration" tab.
Under "Execution role", click on the role name to open it in IAM.
Attach the new policy to the role.


7. Testing the Lambda Function
Configure a test event for the Lambda function.
In Lambda, go to your function.
Click "Test".
Create a new test event with the following JSON:


json
Copy code
{
  "comment": "Generic test event for scheduled Lambda execution. The function does not use this event data.",
  "test": true
}


Run the Lambda function to ensure it can retrieve files from S3 and send emails via SES.
Click "Test" to execute the function.
Check the CloudWatch logs for any errors and ensure emails are sent.





8. Scheduling Email Sends with EventBridge
Create a new schedule using EventBridge to trigger the Lambda function.

Go to the AWS Management Console.
Navigate to EventBridge.
Click "Create rule".
Enter a name and description.
Select "Event Source" as "Schedule".
Define a schedule expression (e.g., cron(0 8 * * ? *) for 8 AM daily).
Add a target and select your Lambda function.
Test the EventBridge schedule to confirm emails are sent on time.


Verify the rule is enabled.
Monitor CloudWatch logs to ensure the Lambda function is triggered as expected.





Conclusion
FitLife Weekly demonstrates the power of serverless architecture in building scalable and efficient email marketing solutions. By leveraging AWS services, we can automate the process of sending personalized emails, making it easier to manage and maintain large-scale email campaigns.

