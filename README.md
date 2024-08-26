# Real-Time Processing with AWS Kinesis - PoC

This Proof of Concept (PoC) demonstrates how to use AWS Kinesis for real-time processing in a microservices architecture. It simulates a scenario where product updates are streamed into Kinesis, processed by a Lambda function, and updates a backend system (e.g., a product catalog in Magento).

## Folder Structure

- **src/**: Contains the Lambda function code and dependencies.
- **infrastructure/**: Contains the CloudFormation templates to deploy the required AWS resources.
- **README.md**: Instructions on how to deploy and use the PoC.

## Prerequisites

1. **AWS CLI**: Ensure the AWS CLI is installed and configured with your credentials.
2. **AWS Account**: You need an AWS account with permissions to create the necessary resources (S3, Lambda, Kinesis).

## Steps to Deploy

1. **Upload Lambda Code to S3**:
   - Zip the contents of the `src/` folder.
   - Upload the zip file to an S3 bucket.

   - ```zip -r lambda.zip src/```
   - ```aws s3 cp lambda.zip s3://your-s3-bucket/lambda.zip```

2. **Deploy CloudFormation Templates**:
    - Deploy Kinesis
    - ```aws cloudformation create-stack --stack-name kinesis-stream-poc --template-body file://infrastructure/kinesis_stream.yml --capabilities CAPABILITY_IAM```

    - Deploy Lambda
    - ```aws cloudformation create-stack --stack-name lambda-poc --template-body file://infrastructure/lambda.yml --capabilities CAPABILITY_IAM```


2. **Test the Setup**:
    - ```aws kinesis put-record --stream-name product-updates-stream --partition-key "partitionKey" --data '{"product_data": {"id": "123", "name": "New Product", "price": 29.99}}'```

## How It Works
1. Vendor triggers a product update: This could be an update in a product's price, inventory, or description.
2. Data is streamed to Kinesis: The vendor's update triggers an event that sends a record to the product-updates-stream.
3. Kinesis stream processes the data: The data is ingested by Kinesis and passed to the Lambda function.
4. Lambda function processes the data: The Lambda function is triggered by Kinesis, processes the product update, and simulates updating the backend system (e.g., Magento).
5. This architecture ensures that product updates are processed in real-time, maintaining the consistency and accuracy of product data in your system.
