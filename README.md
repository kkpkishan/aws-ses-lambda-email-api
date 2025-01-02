# AWS SES Lambda Email API

This project provides an AWS CloudFormation template to deploy an API Gateway and a Lambda function that enables sending emails via AWS SES. The API requires an API key for authentication and supports sending HTML emails with dynamic content.

## Features

- **Secure Email Sending**: Uses AWS SES for sending emails.
- **API Gateway Integration**: Exposes a REST API endpoint (`POST /prod/ses`) to trigger the Lambda.
- **Environment Variables**: Configurable domain or email for the verified SES identity and an API key.
- **Error Handling**: Includes robust error handling for invalid JSON, missing fields, and unauthorized access.

## Prerequisites

1. **Verified Domain/Email in AWS SES**: Ensure your domain or email is verified in SES before deployment.
2. **AWS CLI Installed**: Install the [AWS CLI](https://aws.amazon.com/cli/) and configure it with appropriate credentials.
3. **CloudFormation Setup**: Ensure you have permissions to deploy CloudFormation stacks.

## Deployment

### 1. Clone the Repository
```bash
git clone https://github.com/kkpkishan/aws-ses-lambda-email-api.git
cd aws-ses-lambda-email-api
```

### 2. Deploy the CloudFormation Template
Use the AWS CLI to deploy the stack:
```bash
aws cloudformation deploy \
    --template-file template.yaml \
    --stack-name ses-lambda-email-api \
    --parameter-overrides VerifiedDomainOrEmail=example.com ApiKey=your-api-key \
    --capabilities CAPABILITY_NAMED_IAM
```

Replace:
- `example.com` with your verified SES domain or email.
- `your-api-key` with a secure API key.

### 3. Test the API
After deployment, find the API Gateway endpoint in the CloudFormation stack outputs.

#### Example Request (using `curl`)
```bash
curl -X POST https://your-api-endpoint/prod/ses \
-H "Content-Type: application/json" \
-d '{
  "apikey": "your-api-key",
  "toaddress": "recipient@example.com",
  "emailtemplete": "<h1>Hello, this is a test email!</h1>"
}'
```

Replace:
- `https://your-api-endpoint/prod/ses` with your API endpoint.
- `your-api-key` with the API key you specified during deployment.
- `recipient@example.com` with the recipient's email address.

### 4. Cleanup
To delete the stack and resources:
```bash
aws cloudformation delete-stack --stack-name ses-lambda-email-api
```

## File Structure

```plaintext
aws-ses-lambda-email-api/
├── README.md           # Project documentation
├── template.yaml       # CloudFormation template
```

## Error Handling

- **400 Bad Request**: Returned for invalid JSON or missing required fields in the request body.
- **403 Forbidden**: Returned if the provided API key does not match.
- **500 Internal Server Error**: Returned for AWS SES or other internal errors.

## Contributions

Contributions are welcome! Feel free to fork the repository and submit a pull request.
