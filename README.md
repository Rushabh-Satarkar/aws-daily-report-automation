AWS Daily Report Automation with Lambda

Project Overview :
This project demonstrates a serverless solution for automating the generation and delivery of daily reports using AWS Lambda, Amazon EventBridge (formerly CloudWatch Events), and Amazon Simple Email Service (SES). It's designed to be cost-effective and operate well within the AWS Free Tier limits for typical usage.

Problem Solved :
Many organizations and individuals need regular, automated summaries of key information (e.g., daily activity logs, system health checks, simple business metrics). Manually generating and sending these reports can be time-consuming and prone to human error. This project provides a robust, automated solution to deliver these reports directly to stakeholders' inboxes without requiring dedicated servers or continuous monitoring.

Architecture Workflow :
The solution leverages a fully serverless architecture on AWS.

graph TD
    A[Amazon EventBridge] -- Triggers (Scheduled Rule) --> B(AWS Lambda Function);
    B -- Calls AWS SDK (Boto3) --> C[Amazon SES];
    C -- Sends Email --> D[Recipient Email Inbox];
    B -- Sends Logs & Metrics --> E[Amazon CloudWatch];

    subgraph AWS Cloud
        A
        B
        C
        E
    end

    subgraph External
        D
    end

    style A fill:#FF9900,stroke:#333,stroke-width:2px,color:#fff;
    style B fill:#FF9900,stroke:#333,stroke-width:2px,color:#fff;
    style C fill:#FF9900,stroke:#333,stroke-width:2px,color:#fff;
    style D fill:#33CCFF,stroke:#333,stroke-width:2px,color:#fff;
    style E fill:#FF9900,stroke:#333,stroke-width:2px,color:#fff;

Workflow Explanation:

Amazon EventBridge: A scheduled rule (e.g., daily at 9:00 AM UTC) triggers the Lambda function.

AWS Lambda Function: Executes Python code to generate the report content. In a real-world scenario, this function would interact with other data sources (databases, APIs, S3).

Amazon SES: The Lambda function uses the AWS SDK to send the generated report as an email.

Recipient Email Inbox: The automated report is delivered to the specified recipient(s).

Amazon CloudWatch: Automatically collects logs and metrics from the Lambda function, providing insights into its execution and any potential errors.

AWS Services Used :
AWS Lambda: Serverless compute service for running the report generation code.

Amazon EventBridge: Event bus service used for scheduling the Lambda function.

Amazon Simple Email Service (SES): Cloud-based email sending service for delivering reports.

AWS Identity and Access Management (IAM): For managing permissions for the Lambda function.

Amazon CloudWatch: For monitoring Lambda function execution and logging.

Key Features
Automated Scheduling: Reports are generated and sent automatically at a predefined interval (e.g., daily).

Serverless Architecture: No servers to provision, manage, or patch, reducing operational overhead.

Cost-Effective: Designed to operate within the generous AWS Free Tier limits for small-scale usage.

Scalable: Automatically scales to handle varying loads, though for a daily report, this is less critical.

Customizable Content: The Lambda function can be easily modified to fetch data from various sources and format reports as needed.

Setup and Deployment
To deploy this project to your AWS account, follow these high-level steps. Detailed instructions are available in the project's development guide.

Verify Email Addresses in Amazon SES:

Verify the sender email address (e.g., your-verified-sender@example.com).

If your account is in the SES Sandbox, also verify all recipient email addresses.

Ensure SES is configured in the same AWS Region where you plan to deploy Lambda.

Create AWS Lambda Function:

Create a new Lambda function (e.g., DailyWorkspaceReportFunction) with Python 3.9 runtime.

Configure IAM Role: Attach the AmazonSESFullAccess policy (or a more restrictive custom policy) to the Lambda function's execution role.

Upload Code: Paste the provided lambda_function.py code into the Lambda console's code editor.

Set Environment Variables: Configure SENDER_EMAIL and RECIPIENT_EMAILS environment variables with your verified email addresses.

Set up EventBridge Trigger:

Add an EventBridge trigger to your Lambda function.

Create a new rule with a Schedule expression (e.g., cron(0 9 * * ? *) for daily at 9:00 AM UTC).

Test and Monitor:

Manually test the Lambda function from the console.

Monitor execution logs and metrics in Amazon CloudWatch.

Usage
Once deployed and configured, the Lambda function will automatically run according to your EventBridge schedule. The specified recipient(s) will receive an email with the daily report content.

AWS Free Tier Considerations
This project is designed with the AWS Free Tier in mind:

AWS Lambda: The free tier includes 1 million requests and 400,000 GB-seconds of compute time per month, which is ample for a daily report.

Amazon SES: Offers a free tier for sending a significant number of emails per month (e.g., 62,000 messages when sent from an EC2 instance or Lambda function to SES verified identities).

Amazon EventBridge: The free tier covers a large number of events, making scheduled triggers very cost-effective.

Amazon CloudWatch: Provides a free tier for logs and metrics, sufficient for basic monitoring.

Important: Always monitor your AWS Billing Dashboard to track your usage and ensure you stay within the Free Tier limits. Remember to clean up resources if you no longer need them.

Security Notes
Least Privilege: In a production environment, always apply the principle of least privilege to IAM roles. Instead of AmazonSESFullAccess, create a custom policy allowing only ses:SendEmail.

Sensitive Data: For truly sensitive information (e.g., API keys for external services), use AWS Secrets Manager instead of environment variables.

Code in Public Repositories: NEVER commit AWS access keys, secret keys, or other credentials directly into your code or public GitHub repositories. Use .gitignore to prevent accidental commits of sensitive files.

Future Enhancements
Dynamic Report Content: Integrate with other AWS services (e.g., Amazon DynamoDB, S3, RDS) or external APIs to fetch real-time data for the report.

Advanced Formatting: Use Python libraries like pandas for data processing and Jinja2 for more complex HTML email templates.

Error Notifications: Implement more robust error handling within the Lambda function and send alerts to a dedicated channel (e.g., Slack via SNS) if report generation fails.

User Interface: Create a simple web interface using Amazon API Gateway and AWS Lambda to allow users to trigger reports on demand or configure settings.

Multi-Recipient Management: Store recipient lists in a database (e.g., DynamoDB) to manage them dynamically.

Feel free to fork this repository and adapt it to your specific workspace automation needs!