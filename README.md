# Sports Notification System

## Overview

This system utilizes an event-driven architecture to send notifications to users (via SMS or email) at selected intervals, informing them of the scores of ongoing games.

## Technologies Used
- API: SportsData.io
- AWS Services:
    - AWS Lambda: Used to create the Python script that fetches sports data from the API.
    - Amazon SNS: A publish-subscribe service that sends game score information to users who are subscribed via email or phone number.
    - Amazon EventBridge: Provides an automated schedule that triggers the Lambda function at specified intervals.
- Programming Language: Python

  
![Untitled Diagram-Page-2 drawio](https://github.com/user-attachments/assets/1053310d-e398-4a7b-bf8c-c99eef925be4)

## Features
- Fetches live sports scores from an external API.
- Schedules automated updates using Amazon EventBridge.
- Sends formatted data to subscribers using SNS topics.
- Implements security using the principle of least privilege to provide access for IAM roles.

# IN THE AWS MANAGEMENT CONSOLE: 
## CREATE SNS TOPIC
- Create an SNS topic named sports-topic with a standard message queue type.
- Test the topic by subscribing your email address or phone number.
- Confirm the subscription.
- Any messages published to this SNS topic will automatically be sent to the subscribed email address or phone number.

![Screenshot 2025-01-08 105239](https://github.com/user-attachments/assets/cd4eef79-3505-438f-88a8-1f483ea7ad07)

## CREATE A PUBLISH POLICY FOR SNS
- Navigate to IAM > Policies > Create policy > SNS
- Click json and paste the json in policies/gd-sns-policy.json.
  - _This script allows the entity to which the policy is attached to publish information to the specified resource_
  - Replace the value of Recource with the ARN of the SNS topic created earlier
- Replace REGION and ACCOUNT_ID with your AWS region and account ID.
- Enter a name for the policy click create policy.

## CREATE AN IAM ROLE FOR LAMBDA
This role gives lambda permissions to publish to the SNS topic
- Navigate to IAM > Roles > Create Role
- Select AWS Service and choose Lambda.
- Attach the following policies:
    - SNS Publish Policy (gd_sns_policy) (created in the previous step).
    - Lambda Basic Execution Role (AWSLambdaBasicExecutionRole) (an AWS managed policy).
- Skip adding tags then review and add a name for your role.
- Save the ARN (Amazon resource name) of the role for use in the Lambda function.

## CREATE A LAMBDA FUNCTION
- Navigate to the Lambda service and click Create function.
- Select Author from scratch and enter a name for the function.
- Add the Lambda role you created previously and select Python as the runtime.
- Copy the content of src/notification-system.py from your repository into the code editor on Lambda.
- Go to [SportsDataIO], subscribe, and fetch your API key.
- Navigate to the Configuration tab and create two environment variables:
    - NBA_API_KEY: Your API key.
    - SNS_TOPIC_ARN: The ARN of the SNS topic created earlier.

## SCHEDULE AUTOMATION WITH EVENTBRIDGE
- Navigate to the Eventbridge service in the AWS Management Console.
- Go to Rules → Create Rule.
- Select Event Source: Schedule.
- Set the cron schedule for when you want updates (e.g., hourly).
    e.g 0 9-23/2,0-2/2 * * ? * => will run every 2 hours from 9 AM to 11 PM and every 2 hours from midnight to 2 AM.
- Under Targets, select the Lambda function (gd_notifications) and save the rule.

## FUTURE ENHANCEMENT
- Add NFL score alerts for extended functionality.
- Implement a web UI
- Store user preferences (teams, game types) in DynamoDB for personalized alerts.

