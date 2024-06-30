### Option 1: AWS CodeCommit or GitHub

+ You have two options to manage the source code for this module: AWS CodeCommit (included in the AWS Free Tier) or GitHub.
+ In this tutorial, we will use CodeCommit to store our application code, but you can achieve the same result by creating a repository on GitHub.

### Step 1: Install AWS CLI (if necessary)

+ If you have never configured the AWS CLI on your local machine, open a terminal window to install the AWS CLI.
+ The installation instructions vary depending on the operating system you are using.
+ If you already have the AWS CLI installed and configured, skip to Step 2.

### Step 2: Create a Repository in AWS CodeCommit

+ Open the AWS CodeCommit console.
Choose "Create Repository".
Enter "wildrydes-site" for the repository name.
Choose "Create".

### Step 3: Set up an IAM User with Git Credentials

Once the repository is created, set up an IAM user with Git credentials in the IAM console.
Follow the instructions for Step 1 through Step 3 on the "Setup for HTTPS users using Git credentials" page of the AWS CodeCommit User Guide.
Important Note

When setting up your user in the IAM console, you will need to set up and save two sets of credentials to refer back to:
Create access keys in the IAM > Security Credentials tab. Download the access key and secret access key IDs or copy and save them in a secure location.
Generate HTTPS Git credentials for AWS CodeCommit. Download or save these generated credentials as well.

### Step 4: Configure AWS CLI

In the terminal window you used to install the AWS CLI, enter the command: aws configure
Enter the AWS access key ID and secret access key you created in Step 6.
For "Default region name", enter the region you initially selected to create your CodeCommit repository in.
Leave "Default output format" blank, and press Enter.
