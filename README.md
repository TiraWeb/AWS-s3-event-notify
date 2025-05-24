# Simple AWS Event-Driven Image Processing Notification


This project demonstrates a simple event-driven architecture on AWS. When a new image is uploaded to an S3 bucket, an AWS Lambda function is triggered. This function processes the event and sends a notification via Amazon SNS to a subscribed email address.


## Architecture

The flow of the application is as follows:

1.  **User/Process uploads an image** to a designated S3 Bucket.
2.  **S3 Event Notification** is configured on the bucket for `s3:ObjectCreated:*` events.
3.  This event **triggers an AWS Lambda function**.
4.  The **Lambda function** extracts metadata from the event (e.g., bucket name, object key).
5.  The Lambda function **publishes a message** to an Amazon SNS Topic.
6.  The **SNS Topic** has an email subscription.
7.  The subscribed **email address receives a notification** about the new image upload.

**Diagram:**

![image](https://github.com/user-attachments/assets/c287f3d1-6f6e-4c37-954b-939a7bb8cd4b)

## Technologies Used

*   **Amazon S3 (Simple Storage Service):** For storing the uploaded images and triggering events.
*   **AWS Lambda:** For serverless compute to process the S3 event and send SNS notifications. (Specify your runtime, e.g., Python 3.9)
*   **Amazon SNS (Simple Notification Service):** For sending notifications to subscribed endpoints (in this case, email).
*   **AWS IAM (Identity and Access Management):** For creating roles and policies to grant necessary permissions to Lambda.

## Project Setup & Configuration

Follow these steps to replicate the project:

### 1. S3 Bucket Setup

*   **Create an S3 Bucket:**
    *   Choose a globally unique name (e.g., `your-unique-name-image-uploads`).
    *   Select your desired AWS Region.
    *   Keep default settings for public access (Block all public access is recommended for this project)
      
       ![image](https://github.com/user-attachments/assets/2ce546e9-a437-47e2-8093-02f01c97e337)


### 2. IAM Role for Lambda

*   Create an IAM Role that your Lambda function will assume.
*   Navigate to **IAM** -> **Roles** -> **Create role**.
*   **Trusted entity type:** `AWS service`.
*   **Use case:** `Lambda`.
*   **Permissions policies:**
    1.  `AWSLambdaBasicExecutionRole`: Allows Lambda to write logs to CloudWatch.
    2.  Add a custom inline policy with "S3-full-access" and "SNS-full-access" (only "S3 Read Access" and "SNS Publish Access" for more security)

    ![image](https://github.com/user-attachments/assets/39719bd3-1ec7-4a36-a48a-8cbc53e77a9d)


### 3. SNS Topic & Subscription

*   **Create an SNS Topic:**
    *   Navigate to **Amazon SNS** -> **Topics** -> **Create topic**.
    *   **Type:** `Standard`.
    *   **Name:** e.g., `ImageUploadNotifications`.
    *   Keep other settings as default.
  
       ![image](https://github.com/user-attachments/assets/82263e19-d324-4556-a052-36175c1bccf8)

*   **Create an Email Subscription:**
    *   Once the topic is created, go to the **Subscriptions** tab for your topic.
    *   Click **Create subscription**.
    *   **Protocol:** `Email`.
    *   **Endpoint:** Enter your email address where you want to receive notifications.
    *   Click **Create subscription**.
    *   **Confirm the subscription:** You will receive an email from AWS Notifications. Click the confirmation link in the email.
  
      ![image](https://github.com/user-attachments/assets/a220f872-e856-41c8-9de1-37dc7fbb67fb)


### 4. Lambda Function Setup

*   **Create Lambda Function:**
    *   Navigate to **AWS Lambda** -> **Functions** -> **Create function**.
    *   Select **Author from scratch**.
    *   **Function name:** e.g., `ImageUploadProcessor`
    *   **Runtime:** Choose your preferred runtime (e.g., `Python 3.9`). The code provided is for Python.
    *   **Architecture:** `x86_64`
    *   **Permissions:** Choose `Use an existing role` and select the IAM role you created in Step 2
    *   Click **Create function**.
*   **Lambda Code:**
    *   Copy the code from `lambda_function.py` (or your equivalent file) into the inline code editor or upload it as a .zip file.
      
      ![image](https://github.com/user-attachments/assets/b6987210-b268-4ec1-ae18-84e77951c093)

*   **Configure Trigger :**
    *   In the Lambda function configuration, click **+ Add trigger**.
    *   Select **S3**.
    *   **Bucket:** Choose your S3 bucket.
    *   **Event type:** `All object create events`.
    *   Mark the recursive invocation warning
    *   Click **Add**
      
      ![image](https://github.com/user-attachments/assets/3124888f-6e4e-4f7a-97a9-7544ede715cd)

##
##

Now, upload an object to the s3 bucket and see how it sends a notification to the email!! 😃

![image](https://github.com/user-attachments/assets/21536ca4-e2f9-4abe-acbc-0e3dce6c5f6e)
