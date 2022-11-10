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
* But before you can configure a GitHub action, you need to create a GitHub repo for the Lambda. <p>This is <a href="https://docs.github.com/en/get-started/quickstart/create-a-repo?tool=cli" title="Github repo ">
 how to create a Github repo</a> </p>

* Then create a new node project (and push the code to the Github repo when you’re done).<p> 1. mkdir my-function <br/> 2. cd my-function <br/> 3. npm init -y <br/> <p> All you need for now is one JS-file in this repo: index.js. You can use the code generated for your Lambda but make some minor changes so you can see that the code actually changes when you deploy it. This is my suggestion, feel free to copy/paste it: <br/>
</p> </p><p>Here is an example of AppleScript:</p>

<pre><code>exports.handler = async (event) => {
  const response = {
    statusCode: 200,
    body: JSON.stringify("Hello from Lambda and Github!"),
  };
  return response;
};
</code></pre>
<p> This code will always send a JSON response containing a string with a nice message.</p>


#### <p> N.B. You can later easily extend this app with NPM-dependencies because you have initialized it as an NPM app containing a package.json file.</p> #### 

### Deploy the Lambda function from Github with Github Actions ###

1. You are going to use<a href="https://github.com/features/actions" title="Github Actions">
 Github Actions </a> for this. Github Actions is a CI/CD that you can use for building, testing and deploying your code. The nice thing with this is that it’s built into GitHub. You don’t need to signup for another service for this. It’s actually already there, but you might not have noticed. Go to the page of you repo in GitHub and you’ll see an “Actions” tab.

2. In the Action page, you can get started with a bunch of pre-built workflows. No workflow does exactly what we wanna do, so press the “set up a workflow yourself” button.
3. Now, you’ll see a page where you can edit a file called main.yml inside the .github/workflows/ folder. You could also create this file from your editor and commit it if you prefer.
4. Remove the template code and replace it with the following (I’ll describe what it does after the snippet):<p>Here is an example of AppleScript:</p>

<pre><code>name: deploy to lambda
on: [push]
jobs:
  deploy_source:
    name: build and deploy lambda
    strategy:
      matrix:
        node-version: [16.x]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - name: npm install and build
        run: |
          npm ci
          npm run build --if-present
        env:
          CI: true
      - name: zip
        uses: montudor/action-zip@v0.1.0
        with:
          args: zip -qq -r ./bundle.zip ./
      - name: default deploy
        uses: appleboy/lambda-action@master
        with:
          aws_access_key_id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws_secret_access_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws_region: eu-west-1 // please use cearfully in your region 
          function_name: my-function // this is your lemda function name 
          zip_file: bundle.zip
</code></pre>
## Ok, what does all this code do? Let’s break it down. ##
1. First, we define the name of this action. You’ll use this to identify the workflow if you have many. 
2. Next, we define trigger with on: [push]. This workflow will run anytime you push code to the repo.
3. We only define one job called build and deploy lambda. It uses node version 16.x and it runs on the latest version of Ubuntu. Every time the build starts you’ll get a fresh install of Ubuntu.
4. The main part of the config is under the steps: key. Each step starts either with a uses: or a name:. I’ll go through all the steps here:  
* actions/checkout@v1 This step checks out the code from a Github repo
* Use Node.js ${{ matrix.node-version }} This step installs node on the fresh Ubuntu machine.
* npm install and build This step runs npm ci and then npm run build. This is useful if you want NPM dependencies in your Lambda in the future.
* zip zip down everything to a file called bundle.zip
* default deploy deploy the newly created bundle.zip to the Lambda named my-function (change the name if you chose a different name when you created your Lambda)
5. In the script we reference secrets.AWS_ACCESS_KEY_ID and secrets.AWS_SECRET_ACCESS_KEY. The reason we don’t just hardcode the keys here is because of security. Github has a built-in system to manage keys that we’ll use. Go to Settings->Secrets in your Github repo and then add the secrets there.
#### But where do we get those secrets from? It’s inside your Amazon account. #### 
<p> Go to <a href="https://console.aws.amazon.com/iam/" title="IAM page in your Amazon account">
 IAM page in your Amazon account </a>. press users, and select your user. Press security credentials. Then use one of your existing access keys and secrets, or press “Create access key” to create a new one.</p>

 ### Then add the keys to your Github Secrets. ### 
 <p>Now you are good to go. Commit and push the changes and the build will automatically start. Go to the Actions tab again to see the status.</p>

 <p> Now when you go back to your AWS Lambda console and refresh it, you’ll see the code from your Github repo in there! And every time you push new code, it’ll be auto deployed.</p>

 <p> This app can now be extended to use NPM dependencies, multiple files, unit-tests, and everything you need for a real-world app. What will you create? </p>