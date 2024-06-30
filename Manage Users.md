# Manage Users

## Overview

To handle the accounts of your users, you will build an Amazon Cognito user pool in this module. You'll roll out sites that let users sign in, validate their email addresses, and register as new users.

## Architecture overview

![Serverless_Web_App_LP_assets-03 1403870f0fabeb6a11d3e4116092aa6b19b6a924](https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/73575f81-54c7-433f-96ea-829a36825a6f)

1. Users will create new user accounts when they first visit your website. 

2. Users will receive a confirmation email with a verification number from Amazon Cognito after submitting their registration.

3. Users will come back to your website and provide their email address and verification code in order to validate their account.

4. Users will be allowed to log in once their accounts have been verified, either manually through the interface or through the email verification process.
   
6. Following user login, an Amazon Cognito connection is made by a JavaScript function, which then authenticates via the Secure Remote Password protocol (SRP) and returns a set of JSON Web Tokens (JWT).

7. The JWT contains the identity of the user and authenticates against the RESTful API you will build with Amazon API Gateway in the next module.


## Create an Amazon Congnite 

# Setting Up an Amazon Cognito User Pool

### Step 1: Create a User Pool

1. Go to the Amazon Cognito console: [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)
2. Click on "Create user pool" in the top right corner of the page.
3. Choose "Create a user pool" from the dropdown menu.

### Step 2: Configure Sign-in Experience

1. On the "Configure sign-in experience" page, you will see the following sections:
   - "Cognito user pool sign-in options"
   - "Provider types"
   - "User name requirements"
2. In the "Cognito user pool sign-in options" section, select "User name" as the sign-in method.
3. Keep the defaults for the "Provider types" section.
4. Do not make any selections in the "User name requirements" section.
5. Click on "Next" at the bottom of the page.

### Step 3: Configure Security Requirements

1. On the "Configure security requirements" page, you will see the following sections:
   - "Password policy mode"
   - "Multi-factor authentication (MFA)"
   - "Other security settings"
2. Keep the "Password policy mode" as "Cognito defaults".
3. You can choose to configure multi-factor authentication (MFA) or choose "No MFA".
4. Keep the other security settings as default.
5. Click on "Next" at the bottom of the page.

### Step 4: Configure Sign-up Experience

1. On the "Configure sign-up experience" page, you will see the following sections:
   - "Sign-up form fields"
   - "Verification message"
2. Keep everything as default.
3. Click on "Next" at the bottom of the page.

### Step 5: Configure Message Delivery

1. On the "Configure message delivery" page, you will see the following sections:
   - "Email provider"
   - "FROM email address"
2. For "Email provider", confirm that "Send email with Amazon SES - Recommended" is selected.
3. In the "FROM email address" field, select an email address that you have verified with Amazon SES.
   - Note: If you don't see the verified email address populating in the dropdown, ensure that you have created a verified email address in the same Region you selected at the beginning of the tutorial.
4. Click on "Next" at the bottom of the page.

### Step 6: Integrate Your App

1. On the "Integrate your app" page, you will see the following sections:
   - "App name"
   - "App client"
2. Name your user pool: `WildRydes`.
3. Under "Initial app client", name the app client: `WildRydesWebApp`.
4. Keep the other settings as default.
5. Click on "Next" at the bottom of the page.

### Step 7: Review and Create

1. On the "Review and create" page, review the settings you have configured.
2. Click on "Create user pool" at the bottom of the page.

### Step 8: Save User Pool ID and Client ID

1. On the "User pools" page, select the "User pool name" to view detailed information about the user pool you created.
2. Copy the "User Pool ID" in the "User pool overview" section and save it in a secure location on your local machine.
3. Select the "App Integration" tab and copy and save the "Client ID" in the "App clients and analytics" section of your newly created user pool.


### Step 9: Open the `config.js` File

1. From your local machine, open the `wildryde-site/js/config.js` file in a text editor of your choice.

### Step 10: Update the Cognito Section

1. Update the `cognito` section of the file with the correct values for the User Pool ID and App Client ID you saved in Steps 8 and 9 in the previous section.
2. The `userPoolID` is the User Pool ID from the User Pool overview section.
3. The `userPoolClientID` is the App Client ID from the App Integration > App clients and analytics section of Amazon Cognito.
4. The `region` should be the AWS Region code where you created your user pool. For example, `us-east-1` for the N. Virginia Region, or `us-west-2` for the Oregon Region.
   - If you're not sure which code to use, you can look at the Pool ARN value on the User Pool overview. The Region code is the part of the ARN immediately after `arn:aws:cognito-idp:`.

### Step 11: Update the `config.js` File

1. The updated `config.js` file should look like the following code:

```javascript
window._config = {
    cognito: {
        userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb
        userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv
        region: 'us-west-2' // e.g. us-east-2
    },
    api: {
        invokeUrl: '' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod',
    }
};
```
### Step 12: Save and Deploy the Modified `config.js` File

1. Save the modified `config.js` file.

2. In your terminal window, navigate to the root directory of your project.

3. Add, commit, and push the file to your Git repository to have it automatically deploy to Amplify Console. Use the following commands:

```sh
$ git add .
$ git commit -m "new_config"
$ git push
```
