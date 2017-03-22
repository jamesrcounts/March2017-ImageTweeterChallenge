# Build a Serverless .NET Core Image Tweeter

In this challenge we're going to learn how to trigger an [AWS Lambda](https://aws.amazon.com/lambda/) function when a file is uploaded to [Amazon S3](https://aws.amazon.com/s3/). That [AWS Lambda](https://aws.amazon.com/lambda/) function will take the uploaded file and tweet the image to a [Twitter](http://www.twitter.com/) account.

## Prerequisites

1. AWS Tools
    1. Sign-up for an [AWS account](https://aws.amazon.com)
    2. Install [AWS CLI](https://aws.amazon.com/cli/)
2. .NET Core
    1. Install [.NET Core 1.0](https://www.microsoft.com/net/core) **(NOTE: You MUST install 1.0. Do NOT install 1.1 or later!)**
    2. Install [Visual Studio Code](https://code.visualstudio.com/)
    3. Install [C# Extension for VS Code](https://code.visualstudio.com/Docs/languages/csharp)
3. AWS C# Lambda Tools
    1. Install [Nodejs](https://nodejs.org/en/)
    2. Install [Yeoman](http://yeoman.io/codelab/setup.html)
    3. Install AWS C# Lambda generator: `npm install -g yo generator-aws-lambda-dotnet`

## Level 1: Trigger an [AWS Lambda](http://aws.amazon.com/lambda/) function from [Amazon S3](http://aws.amazon.com/s3/)

For level 1, you will need to create an [Amazon S3](http://aws.amazon.com/s3/) bucket, create an [AWS IAM](https://aws.amazon.com/iam/) role and policy, deploy an [AWS Lambda](http://aws.amazon.com/lambda/) function and use [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/) to debug and monitor. Use `us-west-2` aka `US West Oregon` region. Below are some hints to help you get started.


**[AWS IAM](https://aws.amazon.com/iam/) role:**

- Create an [AWS IAM](https://aws.amazon.com/iam/) policy for the [AWS Lambda](http://aws.amazon.com/lambda/) function [AWS IAM](https://aws.amazon.com/iam/) role.
	
	- Allow it to create and put objects to [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/).
	- If using the console to create the IAM role, choose `CloudWatchLogFullAccess`.
   - See policy: [aws/policies/lambda_role_policy_level_1.json](aws/policies/lambda_role_policy_level_1.json)
 
- Create an [AWS IAM](https://aws.amazon.com/iam/) role for the [AWS Lambda](http://aws.amazon.com/lambda/) function.

	- Make sure it has a trust relationship with `lambda.amazonaws.com`.
		- If using the console to create the IAM role, choose `AWS Lambda` on `Step 2 : Select Role Type`.
		- If you're curious as to the policy it created for your see: [aws/policies/lambda_role_trust_relationship.json](aws/policies/lambda_role_trust_relationship.json)
	- Attach the policy created above.

**Deploy the [AWS Lambda](http://aws.amazon.com/lambda/) starter function:**

- Run the following in terminal as you need, from the `ImageTweeter` directory.

	- To refresh dependencies: `dotnet restore`
	- To build the lambda function: `dotnet build`
	- To publish the lambda function: `dotnet publish`
	- To deploy the lambda function: `dotnet lambda deploy-function -fn lamda-sharp-image-tweeter`

*Note: On first deployment it will ask you for a role. Choose the role you created earlier. If you do not see it in the list, check your Trust Relationship for the role that it allows lambda.amazonaws.com.*

**[Amazon S3](http://aws.amazon.com/s3/) bucket:**

- The bucket name must be unique.
- Add an event on the bucket that triggers the [AWS Lambda](http://aws.amazon.com/lambda/) function.
- For this project you can use images from [aws/images](aws/images) or use your own.

**ACCEPTANCE TEST:** The lambda function is triggered by uploading an image to [Amazon S3](http://aws.amazon.com/s3/). Look in [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/) for log message `Hello from The AutoMaTweeter!`.

## Level 2: Log the bucket and key from the [Amazon S3](http://aws.amazon.com/s3/) event

For level 2, modify the `LambdaHandler.cs` file in `src/ImageTweeter` to read the [Amazon S3 event notification](http://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html). Get the `bucket` and `key` name from the event and output to [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/). 

You can use the imports from the `LambdaHandler.cs` file and the [AWS SDK for .NET](http://docs.aws.amazon.com/sdkfornet/v3/apidocs/Index.html) documentation as a reference.

**ACCEPTANCE TEST:** The [Amazon S3](http://aws.amazon.com/s3/) bucket and key name appear in the [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/) logs.

## Level 3: Post the image uploaded to [Twitter](http://www.twitter.com)

For level 3, modify the code to post the uploaded image to [Twitter](http://www.twitter.com) with a message. [TweetInvi](https://github.com/linvi/tweetinvi/wiki) is the library I used and included it as a depedency in this project. You'll also need to modify the [Amazon S3](http://aws.amazon.com/s3/) bucket and [AWS IAM](https://aws.amazon.com/iam/) policy.

**[Amazon S3](http://aws.amazon.com/s3/) bucket:**

- The bucket needs a policy to allow AWS resources ([Principal](http://docs.aws.amazon.com/AmazonS3/latest/dev/s3-bucket-user-policy-specifying-principal-intro.html)) to get objects from that bucket.
- See starter policy, you will need to replace some values: [aws/policies/lambda_role_policy_level_2.json](aws/policies/s3_bucket_policy.json)

**[AWS IAM](https://aws.amazon.com/iam/) role:**

- Update the policy for the lambda function role to allow it to get objects from [Amazon S3](http://aws.amazon.com/s3/)
- See starter policy, you will need to replace some values: [aws/policies/lambda_role_policy_level_2.json](aws/policies/lambda_role_policy_level_3.json)

**[Twitter](http://twitter.com/) API Keys:**

- You will also need 4 [Twitter](http://www.twitter.com) API keys: consumer key, consumer secret, access token, and access token secret, which I will provide for tonight's project.
- You can view the tweets at: [https://twitter.com/the_automatweet](https://twitter.com/the_automatweet)
- If you'd like keys for your account, you can get them on the [Twitter App](https://apps.twitter.com) site.

*Note: I had some conflicting namespace and functions at times. If you have this issue, my workaround was to create my own namespace `using AmazonRekognitionModel = Amazon.Rekognition.Model;`*

**ACCEPTANCE TEST:** After the image is uploaded to [Amazon S3](http://aws.amazon.com/s3/) it will post to [Twitter](http://www.twitter.com).

## Boss Level: Analyze the image with [Amazon Rekognition](https://aws.amazon.com/rekognition/)

For the boss level, use [Amazon Rekognition](https://aws.amazon.com/rekognition/) to tweet keyword descriptions of the image uploaded to [Amazon S3](http://aws.amazon.com/s3/).

**ACCEPTANCE TEST:** The tweet includes an the image uploaded and a list of words that describe the image.

Have fun!! :smile:
