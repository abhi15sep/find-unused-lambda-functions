# find-unused-lambda-functions
AWS Lambda lets you run code without provisioning or managing servers. With Lambda, you can run code for virtually any type of application or backend service - all with zero administration. 

Understanding which functions are being invoked and which are not can help you maintain an up-to-date Lambda environment and control Lambda costs by removing unused functions from production.

This is a simple python script that uses Amazon Athena and AWS CloudTrail to list the Lambda functions that have not been invoked in the past 30 days. Running this script will create and run an Amazon Athena query on your CloudTrail data. You are charged for the number of bytes scanned by Amazon Athena, rounded up to the nearest megabyte, with a 10MB minimum per query. There are no charges for Data Definition Language (DDL) statements like CREATE/ALTER/DROP TABLE, statements for managing partitions, or failed queries. See [Athena pricing](https://aws.amazon.com/athena/pricing/) for pricing details and examples.

### Prerequisites

This sample project depends on [boto3](https://aws.amazon.com/sdk-for-python/), the AWS SDK for Python, and requires Python 2.6.5+, 2.7, and 3.3+. You can install boto3 using pip:

    pip install boto3
	
The script also requires that you have CloudTrail data events enabled for all Lambda functions within your AWS account. The following blog "[Gain Visibility into the Execution of Your AWS Lambda functions with AWS CloudTrail](https://aws.amazon.com/blogs/mt/gain-visibility-into-the-execution-of-your-aws-lambda-functions-with-aws-cloudtrail/)" provides a step-by-step guide.

### Basic Configuration

Before you can begin using Boto 3, you should set up authentication credentials. Credentials for your AWS account can be found in the IAM Console. You can create or use an existing user. Go to manage access keys and generate a new set of keys.

If you have the AWS CLI installed, then you can use it to configure your credentials file:

	aws configure

Alternatively, you can create the credential file yourself. By default, its location is at ~/.aws/credentials (C:\Users\USER_NAME\.aws\credentials for Windows users). Add the following lines in the file:

[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY

You also need to set a default region. This can be done in the configuration file. By default, its location is at ~/.aws/config (C:\Users\USER_NAME\.aws\config for Windows users). Add the following lines in the file:

[default]
region=us-east-1

See the [Security Credentials](http://aws.amazon.com/security-credentials) page
for more information on getting your keys. For more information on configuring boto3,
check out the Quickstart section in the [developer guide](https://boto3.readthedocs.org/en/latest/guide/quickstart.html).

### Configuring Script Variables

A step by step series of examples that tell you have to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
