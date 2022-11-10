# How to set up an AWS Lambda and auto deployments with Github Actions #

### In this guide, you’ll learn: ###

* How to create a Lambda with AWS console .
* How to write a hello-world app for your Lambda.
* How to configure a Github Action to automaticly deploy the code in your Github Repo to the Lambda whenever you push code.
## Prerequisites to follow along: ##
* Have a GitHub account
* A Github account. A free account is enough for starting. You’ll get 2000 build-minutes of Github Actions per month   which is enough for smaller projects.
* A Github repo 
* Have an AWS account
* Create an AWS IAM user that has the following permissions: IAMFullAccess, AmazonS3FullAccess, CloudWatchFullAccess, AWSCloudFormationFullAccess, AWSLambda_FullAccess and AmazonAPIGatewayInvokeFullAccess.
* Store the AWS user API key and secret key (keep it safe)