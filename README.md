# find-unused-lambda-functions
AWS Lambda lets you run code without provisioning or managing servers. You pay only for the compute time you consume - there is no charge when your code is not running. With Lambda, you can run code for virtually any type of application or backend service - all with zero administration. Just upload your code and Lambda takes care of everything required to run and scale your code with high availability. You can set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app.

Understanding which functions are being invoked and which are not can help you maintain an up-to-date Lambda environment and control Lambda costs by removing unneeded functions from production.

This is a simple python script that uses Amazon Athena and AWS CloudTrail to list the Lambda functions that have not been invoked in the past 30 days. Running this script will create a run an Amazon Athena query on your CloudTrail data. You are charged for the number of bytes scanned by Amazon Athena, rounded up to the nearest megabyte, with a 10MB minimum per query. There are no charges for Data Definition Language (DDL) statements like CREATE/ALTER/DROP TABLE, statements for managing partitions, or failed queries. See [Athena pricing](https://aws.amazon.com/athena/pricing/) for pricing details and examples.

### Prerequisites

This sample project depends on [boto3](https://aws.amazon.com/sdk-for-python/), the AWS SDK for Python, and requires Python 2.6.5+, 2.7, and 3.3+. You can install `boto3` using pip:

    pip install boto3
	
The script also requires that you have CloudTrail data events enabled for all Lambda functions within your AWS account. The following blog [Gain Visibility into the Execution of Your AWS Lambda functions with AWS CloudTrail](https://aws.amazon.com/blogs/mt/gain-visibility-into-the-execution-of-your-aws-lambda-functions-with-aws-cloudtrail/) provides a step-by-step guide.

## Basic Configuration

You need to set up your AWS security credentials before the sample code is able
to connect to AWS. You can do this by creating a file named "credentials" at ~/.aws/ 
(`C:\Users\USER_NAME\.aws\` for Windows users) and saving the following lines in the file:

    [default]
    aws_access_key_id = <your access key id>
    aws_secret_access_key = <your secret key>

See the [Security Credentials](http://aws.amazon.com/security-credentials) page
for more information on getting your keys. For more information on configuring `boto3`,
check out the Quickstart section in the [developer guide](https://boto3.readthedocs.org/en/latest/guide/quickstart.html).
```

### Configuring the Variables

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
