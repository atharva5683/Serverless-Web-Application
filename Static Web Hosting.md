# Static Web Hosting with Continuous Deployment

## Overview
You will set up Amplify to host the static resources for your web application with integrated continuous deployment in this module. A git-based process for full-stack web application hosting and continuous deployment is offered via the Amplify Console. You will add dynamic functionality in later modules by calling distant RESTful APIs constructed with Lambda and API Gateway using JavaScript.

## Architecture overview

![Amplify_Wild_Rydes 1760839c5336d01cd6ac6eabb5d2ad8a37c3304a](https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/c53d6b88-3ce0-4fb7-b377-aeebbe59b277)

All of your static web content including HTML, CSS, JavaScript, images, and other files will be managed by AWS Amplify Console. Your end users will then access your site using the public website URL exposed by AWS Amplify Console.

## Create Git Repo 

### Option 1: AWS CodeCommit or GitHub

+ You have two options to manage the source code for this module: AWS CodeCommit (included in the AWS Free Tier) or GitHub.
+ In this tutorial, we will use CodeCommit to store our application code, but you can achieve the same result by creating a repository on GitHub.

### Step 1: Install AWS CLI (if necessary)

+ If you have never configured the AWS CLI on your local machine, open a terminal window to install the AWS CLI.
+ The installation instructions vary depending on the operating system you are using.
+ If you already have the AWS CLI installed and configured, skip to Step 2.

### Step 2: Create a Repository in AWS CodeCommit

+ Open the AWS CodeCommit console.
+ Choose "Create Repository".
+ Enter "wildrydes-site" for the repository name.
+ Choose "Create".

### Step 3: Set up an IAM User with Git Credentials

+ Once the repository is created, set up an IAM user with Git credentials in the IAM console.
+ Follow the instructions for Step 1 through Step 3 on the "Setup for HTTPS users using Git credentials" page of the AWS CodeCommit User Guide.

__Important Note__

When setting up your user in the IAM console, you will need to set up and save two sets of credentials to refer back to:
Create access keys in the IAM > Security Credentials tab. Download the access key and secret access key IDs or copy and save them in a secure location.
Generate HTTPS Git credentials for AWS CodeCommit. Download or save these generated credentials as well.

### Step 4: Configure AWS CLI

+ In the terminal window you used to install the AWS CLI, enter the command: `aws configure`
+ Enter the AWS access key ID and secret access key you created .
+ For "Default region name", enter the region you initially selected to create your CodeCommit repository in.
+ Leave "Default output format" blank, and press Enter.

The following code block is an example of what you will see in your terminal window.

```bash
% aws configure
AWS Access Key ID [****************]: #####################
AWS Secret Access Key [****************]: ###################
Default region name [us-east-1]: us-east-1
Default output format [None]:
```

### Step 5: Set up Git Config Credential Helper

+ Set up the Git config credential helper in the terminal window:

    + If you have a Linux, macOS, or UNIX machine, follow the instructions for Step 3: Set up the credential helper for Linux, macOS, or UNIX.
    + If you have a Windows machine, follow the instructions for Step 3: Set up the credential helper for Windows.
    + Run the following commands in the terminal window:


```bash
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

### Step 6: Clone the Repository

+ Navigate back to the AWS CodeCommit console and select the wildrydes-site repository.
+ Select Clone HTTPS from the Clone URL dropdown to copy the HTTPS URL.

![wildrydes_clone 5ce668af0eea388fd94a993db5ce8365a95acf86](https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/de2f20ae-64d0-464c-96bb-903c11788600)

### Step 7: Clone the Repository using Git

+ From your terminal window, run the following command to clone the repository:

```
git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
```

+ You will be prompted to enter your HTTPS Git credentials for AWS CodeCommit:
     + Username: Enter the HTTPS Git credentials for AWS CodeCommit username you generated in Step 6.
     + Password: Enter the HTTPS Git credentials for AWS CodeCommit password you generated in Step 6.
  
__Note__

You will see a warning message indicating that you appear to have cloned an empty repository. This is expected.
The output in your terminal window should look similar to this:


```
$ git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
Cloning into ‘wildrydes-site’...
Username for ‘https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site’: 
Password for ‘https://username@git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site’: 
warning: You appear to have cloned an empty repository.
```
### Step 8: Copy Website Content from S3 and Add to Repository

+ Copy Website Content from S3
  + Change directory into your repository:

```
cd wildrydes-site
```
  + Copy the static files from the S3 bucket using the following command (make sure to replace "us-east-1" with the Region you selected at the beginning of this tutorial):

```
aws s3 cp s3://wildrydes-<Region>/WebApplication/1_StaticWebHosting/website ./ --recursive
```
+ Add, Commit, and Push the Git Files
   + Add all files to the Git index:
```
git add .
```
  + Commit the changes with a meaningful commit message:
```
git commit -m "new files"
```
  + Push the changes to your Git repository:

```
git push
```
___Example Output___
The output in your terminal window should look similar to this:

```
$ git add .
$ git commit -m "new files"
$ git push
Counting objects: 95, done.
Compressing objects: 100% (94/94), done.
Writing objects: 100% (95/95), 9.44 MiB | 14.87 MiB/s, done.
Total 95 (delta 2), reused 0 (delta 0)
To https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
* [new branch] master -> master
```
Note: Replace  "<Region>" with the actual Region you selected at the beginning of this tutorial.

# Enable Web Hosting with Aws Amplify Console

### Step 1: Launch the AWS Amplify Console

+ Go to the AWS Amplify Console and click on "Get Started".

### Step 2: Choose Amplify Hosting

+ Under the "Amplify Hosting" header, click on "Get Started".

### Step 3: Select Repository

+ On the "Get started with Amplify Hosting" page, select "AWS CodeCommit" and click on "Continue".
+ If you used GitHub, you'll need to authorize AWS Amplify to access your GitHub account.

### Step 4: Add Repository Branch

+ On the "Add repository branch" step, select "wildrydes-site" from the "Select a repository" dropdown.
+ In the "Branch" dropdown, select "master" and click on "Next".

### Step 5: Configure Build Settings

+ On the "Build settings" page, leave all the defaults as they are.
+ Select the option to "Allow AWS Amplify to automatically deploy all files hosted in your project root directory".
+ Click on "Next".

### Step 6: Review and Deploy

+ On the "Review" page, click on "Save and deploy".

### Step 7: Wait for Deployment

+ The process takes a couple of minutes for Amplify Console to create the necessary resources and deploy your code.

### Step 8: Launch Your Website

+ Once completed, click on the site image or the link underneath the thumbnail to launch your Wild Rydes site.
+ If you select the link for "master", you'll see the build and deployment details related to your branch.

<img width="1360" alt="amplify-deploy-status" src="https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/6604e8fe-c212-4840-8ecb-bf2675575617">
![wildrydes_clone4 1ec890e03dc19eebef4d5b3c71e3b3c7afb8e978](https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/a7d7abac-31eb-4079-8937-9317c7ca9825)
