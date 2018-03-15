# find-unused-lambda-functions
AWS Lambda lets you run code without provisioning or managing servers. With Lambda, you can run code for virtually any type of application or backend service - all with zero administration. 

Understanding which functions are being invoked and which are not can help you maintain an up-to-date Lambda environment and control Lambda costs by removing unused functions from production.

This is a simple python script that uses Amazon Athena and AWS CloudTrail to list the Lambda functions that have not been invoked in the past 30 days. Running this script will create and run an Amazon Athena query on your CloudTrail data. You are charged for the number of bytes scanned by Amazon Athena, rounded up to the nearest megabyte, with a 10MB minimum per query. There are no charges for Data Definition Language (DDL) statements like CREATE/ALTER/DROP TABLE, statements for managing partitions, or failed queries. See [Athena pricing](https://aws.amazon.com/athena/pricing/) for pricing details and examples.

## Prerequisites

The Python script supports Python 2.6.5+, 2.7, and 3.3+.

### Boto3 Setup and Configuration

This sample project depends on [boto3](https://aws.amazon.com/sdk-for-python/), the AWS SDK for Python. 

To install Boto3 you can clone this repository and type:

	pip install -r requirements.txt

Or you can install boto3 using pip:

    pip install boto3
	
The script also requires that you have AWS CloudTrail data events enabled for all Lambda functions within your AWS account. The following blog "[Gain Visibility into the Execution of Your AWS Lambda functions with AWS CloudTrail](https://aws.amazon.com/blogs/mt/gain-visibility-into-the-execution-of-your-aws-lambda-functions-with-aws-cloudtrail/)" provides a step-by-step guide. In order for the Python script to return accurate results based on the past 30 day, you’ll need to ensure that CloudTrail data events for Lambda has been enabled for all functions for at least that period of time. 

Before you can begin using Boto 3, you should set up authentication credentials. Credentials for your AWS account can be found in the IAM Console. You can create or use an existing user. Go to manage access keys and generate a new set of keys.

If you have the AWS CLI installed, then you can use it to configure your credentials file using the command:

	aws configure

Alternatively, you can create the credential file yourself. By default, its location is at ~/.aws/credentials (C:\Users\USER_NAME\.aws\credentials for Windows users). Add the following lines in the file:

	[default]
	aws_access_key_id = YOUR_ACCESS_KEY
	aws_secret_access_key = YOUR_SECRET_KEY

See the [Security Credentials](http://aws.amazon.com/security-credentials) page
for more information on getting your keys. For more information on configuring boto3,
check out the Quickstart section in the [developer guide](https://boto3.readthedocs.org/en/latest/guide/quickstart.html).

### AWS Permissions

You’ll need to ensure that the IAM credentials that you are using above have the appropriate permissions to [list your Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-api-permissions-ref.html) and [execute Athena queries](https://docs.aws.amazon.com/athena/latest/ug/access.html#managed-policies).

### Python Variable Configuration

There are 4 variables you'll need to set within the Python script before running. These variables are:

The AWS region that you want to evaluate. Its important to note that the script will only work in regions where Athena is supported. Athena region availability can be found at [https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/). The region designation should be set as the region code. For example, US East (N. Virginia) is 'us-east-1'.

	REGION = "us-east-1"

Name of the S3 bucket where Athena will store the query history when running the script. This bucket will be created in the region where the script is executed if it doesn't currently exist. Example:

	ATHENA_S3_BUCKET_NAME = "s3://athena-history-bucket-demo"

Name of the Athena table to create for CloudTrail logs. This table will be created in the 'default' Athena database. Example:

	TABLE_NAME = "cloudtrail_logs"

Location of S3 bucket where CloudTrail logs are stored for your CloudTrail Lambda data events. You can find this location by viewing the CloudTrail trail and copying the S3 bucket where log files are delivered. This is in the format of s3://{BucketName}/AWSLogs/{AccountID}/. Example:

	CLOUDTRAIL_S3_BUCKET_NAME = "s3://cloudtrail-logs-bucket/AWSLogs/123456789012/"

## Running the Script

With all the prerequisites met and the variables configured, you can now run the script. Before running it’s important to understand that while there is no cost for the script to create the CloudTrail table within Athena, the step of running an actual Athena query to search for Lambda invocations will incur standard Athena charges. Please visit the [Athena pricing](https://aws.amazon.com/athena/pricing/) page for more information.

To run the script:

	python unusedlambda.py

This will perform the following action in order:
1.	Retrieve a list of the current Lambda functions found in the region specified in the configuration file or set using the AWS CLI. The script will output the total count of functions found.

The next 3 steps will run a set of queries within Athena. When each query is launched you’ll see a Query Execution ID and the script will return a “Running” every 5 seconds waiting for the query results to return. 

2.	Create an Athena table with the name specified in the 'TABLE_NAME' variable. This table is created using the AWS CloudTrail SerDe and specific DDL required to query AWS CloudTrail logs. 

3.	Create a partition in the newly created CloudTrail table for the year 2018. This limits the amount of data that Amazon Athena will need to query to return the results within the past 30 days.

4.	Using the list of functions retrieved in Step 1, create and execute an Amazon Athena query that returns a list of which functions have been invoked in the past 30 days.

Note: If you run the script more than once within the same region, using the same set of script variables, you’ll notice that the queries for Step 2 and Step 3 will return failed results. This is expected behavior as the CloudTrail table and partition already exist.

5.	Finally, the script will output the difference between the list of functions that exist within the region and the query results.

The output represents all the functions within the designated regions which have *NOT* been called in the past 30 days.
