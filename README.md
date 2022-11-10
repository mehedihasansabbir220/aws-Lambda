# How to set up an AWS Lambda and auto deployments with Github Actions #

### In this guide, you’ll learn: ###

* How to create a Lambda with AWS console .
* How to write a hello-world app for your Lambda.
* How to configure a Github Action to automaticly deploy the code in your Github Repo to the Lambda whenever you push code.
## Prerequisites to follow along: ##
1. Have a GitHub account
2. A Github account. A free account is enough for starting. You’ll get 2000 build-minutes of Github Actions per month   which is enough for smaller projects.
3. A Github repo 
4. Have an AWS account
5. Create an AWS IAM user that has the following permissions: IAMFullAccess, AmazonS3FullAccess, CloudWatchFullAccess, AWSCloudFormationFullAccess, AWSLambda_FullAccess and AmazonAPIGatewayInvokeFullAccess.
6. Store the AWS user API key and secret key (keep it safe)

### Set up the Lambda with AWS console ### 

1. First, go to the Lambda page inside AWS by searching for Lambda.
2. Then, create a new function. (a Lambda is a function)
3. Now give the function a name. I chose my-function. You don’t have to do any other changes, just use the defaults.<br/> Now a new function is created and you are redirected to the console page for your newly created Lambda function.
4. Optional :<br/> You can test the function by pressing the test button in the upper right. This is the only way to run the function right now.


### Create the Github Repo that you’re going to deploy from ###
> But before you can configure a GitHub action, you need to create a GitHub repo for the Lambda. <p>This is <a href="https://docs.github.com/en/get-started/quickstart/create-a-repo?tool=cli" title="Github repo ">
 how to create a Github repo</a>
 </p>