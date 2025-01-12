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


## CREATE SNS TOPIC
- Create an SNS topic named sports-topic with a standard message queue type.
- Test the topic by subscribing your email address or phone number.
- Confirm the subscription.
- Any messages published to this SNS topic will automatically be sent to the subscribed email address or phone number.

![Screenshot 2025-01-08 105239](https://github.com/user-attachments/assets/cd4eef79-3505-438f-88a8-1f483ea7ad07)

## CREATE A PUBLISH POLICY FOR SNS
- This IAM role will give the lambda functions permissions to enable it to publish to the SNS topic.
- Navigate to IAM > Policies > Create policy.
- Click json and paste the json in policies/gd-sns-policy.json.
    _This script allows the entity to which the policy is attached to publish information to the specified resource_
- Replace REGION and ACCOUNT_ID with your AWS region and account ID.
- Enter a name for the policy click create policy.



