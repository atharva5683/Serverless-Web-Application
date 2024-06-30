# Serverless Backend

## Overview

You will create a backend procedure for managing requests for your web application in this module by using Amazon DynamoDB and AWS Lambda. Users can request the location of a unicorn to be sent to by using the browser application that you published in the first module. JavaScript in the browser must call a cloud-based service in order to respond to such requests.

## Architecture overview

![Serverless_Web_App_LP_assets_04 76030d61413ff43bd6aa75fbd16e02ad23aec67a](https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/0e52d536-28e9-43ea-a8e8-0d5aafc736c1)

For every unicorn request made by a user, a Lambda function is called. After choosing a unicorn from the fleet and logging the request in a DynamoDB table, the function will reply to the front-end application with information on the unicorn's dispatch.

The browser uses Amazon API Gateway to call the function. That link will be put into practice in the following module. You will test your function independently for this module.

## Create An Amazon DynamoDB Table 

# Creating a DynamoDB Table

## Step 1: Access the Amazon DynamoDB Console

1. Go to the Amazon DynamoDB console.

## Step 2: Create a New Table

1. Click on "Create table" in the DynamoDB console.

## Step 3: Enter Table Details

1. In the "Create table" page, enter the following details:
   - **Table name**: Enter `Rides` (case sensitive).
   - **Partition key**: Enter `RideId` (case sensitive) and select "String" as the key type.

## Step 4: Configure Table Settings

1. In the "Table settings" section, ensure that "Default settings" is selected.
2. Click on "Create table" to create the table.

## Step 5: Wait for Table Creation to Complete

1. On the "Tables" page, wait for the table creation to complete. The status will change to "Active" once the table is created.

## Step 6: Select the Table and Copy the ARN

1. Select the newly created table by clicking on the table name.
2. In the "Overview" tab, go to the "General Information" section.
3. Click on "Additional info" and copy the ARN (Amazon Resource Name) of the table. You will need this ARN in the next section.

That's it! You have now created a new DynamoDB table named `Rides` with a partition key of `RideId` and copied the ARN of the table.


## Create IAM role for lambda fucation 

# Creating an IAM Role for Lambda with DynamoDB Access

## Step 1: Access the IAM Console

1. Go to the IAM console.

## Step 2: Create a New Role

1. In the left navigation pane, select "Roles" and then choose "Create Role".

## Step 3: Define the Trusted Entity Type

1. In the "Trusted Entity Type" section, select "AWS service".
2. For "Use case", select "Lambda" and then choose "Next".

## Step 4: Attach the `AWSLambdaBasicExecutionRole` Policy

1. Enter `AWSLambdaBasicExecutionRole` in the filter text box and press Enter.
2. Select the checkbox next to the `AWSLambdaBasicExecutionRole` policy name and choose "Next".

## Step 5: Enter Role Details

1. Enter `WildRydesLambda` for the "Role Name".
2. Keep the default settings for the other parameters.
3. Choose "Create Role".

## Step 6: Add Inline Policy for DynamoDB Access

1. On the "Roles" page, type `WildRydesLambda` in the filter box and select the name of the role you just created.
2. On the "Permissions" tab, under "Add permissions", choose "Create Inline Policy".
3. In the "Select a service" section, type `DynamoDB` into the search bar and select `DynamoDB` when it appears.
4. Choose "Select actions".
5. In the "Actions allowed" section, type `PutItem` into the search bar and select the checkbox next to `PutItem` when it appears.
6. In the "Resources" section, with the "Specific" option selected, choose the "Add ARN" link.
7. Select the "Text" tab. Paste the ARN of the table you created in DynamoDB (from Step 6 in the previous section) and choose "Add ARNs".
8. Choose "Next".
9. Enter `DynamoDBWriteAccess` for the policy name and choose "Create policy".

That's it! You have now created an IAM role named `WildRydesLambda` that grants your Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB table.


## Create LAmbda Function 

# Creating and Configuring a Lambda Function

## Step 1: Access the AWS Lambda Console

1. Go to the AWS Lambda console.

## Step 2: Create a New Lambda Function

1. Choose "Create a function".
2. Keep the default "Author from scratch" card selected.

## Step 3: Enter Function Details

1. Enter `RequestUnicorn` in the "Function name" field.
2. Select `Node.js 16.x` for the "Runtime" (note: newer versions of Node.js will not work in this tutorial).

## Step 4: Configure the Execution Role

1. Select "Use an existing role" from the "Change default execution role" dropdown.
2. Select `WildRydesLambda` from the "Existing Role" dropdown.

## Step 5: Create the Function

1. Click on "Create function".

## Step 6: Replace the Code

1. Scroll down to the "Code source" section.
2. Replace the existing code in the index.js code editor with the contents of `requestUnicorn.js`.

Here is the `requestUnicorn.js` code:

```javascript
const randomBytes = require('crypto').randomBytes;
const AWS = require('aws-sdk');
const ddb = new AWS.DynamoDB.DocumentClient();

const fleet = [
    {
        Name: 'Angel',
        Color: 'White',
        Gender: 'Female',
    },
    {
        Name: 'Gil',
        Color: 'White',
        Gender: 'Male',
    },
    {
        Name: 'Rocinante',
        Color: 'Yellow',
        Gender: 'Female',
    },
];

exports.handler = (event, context, callback) => {
    if (!event.requestContext.authorizer) {
      errorResponse('Authorization not configured', context.awsRequestId, callback);
      return;
    }

    const rideId = toUrlString(randomBytes(16));
    console.log('Received event (', rideId, '): ', event);

    // Because we're using a Cognito User Pools authorizer, all of the claims
    // included in the authentication token are provided in the request context.
    // This includes the username as well as other attributes.
    const username = event.requestContext.authorizer.claims['cognito:username'];

    // The body field of the event in a proxy integration is a raw string.
    // In order to extract meaningful values, we need to first parse this string
    // into an object. A more robust implementation might inspect the Content-Type
    // header first and use a different parsing strategy based on that value.
    const requestBody = JSON.parse(event.body);

    const pickupLocation = requestBody.PickupLocation;

    const unicorn = findUnicorn(pickupLocation);

    recordRide(rideId, username, unicorn).then(() => {
        // You can use the callback function to provide a return value from your Node.js
        // Lambda functions. The first parameter is used for failed invocations. The
        // second parameter specifies the result data of the invocation.

        // Because this Lambda function is called by an API Gateway proxy integration
        // the result object must use the following structure.
        callback(null, {
            statusCode: 201,
            body: JSON.stringify({
                RideId: rideId,
                Unicorn: unicorn,
                Eta: '30 seconds',
                Rider: username,
            }),
            headers: {
                'Access-Control-Allow-Origin': '*',
            },
        });
    }).catch((err) => {
        console.error(err);

        // If there is an error during processing, catch it and return
        // from the Lambda function successfully. Specify a 500 HTTP status
        // code and provide an error message in the body. This will provide a
        // more meaningful error response to the end client.
        errorResponse(err.message, context.awsRequestId, callback)
    });
};

// This is where you would implement logic to find the optimal unicorn for
// this ride (possibly invoking another Lambda function as a microservice.)
// For simplicity, we'll just pick a unicorn at random.
function findUnicorn(pickupLocation) {
    console.log('Finding unicorn for ', pickupLocation.Latitude, ', ', pickupLocation.Longitude);
    return fleet[Math.floor(Math.random() * fleet.length)];
}

function recordRide(rideId, username, unicorn) {
    return ddb.put({
        TableName: 'Rides',
        Item: {
            RideId: rideId,
            User: username,
            Unicorn: unicorn,
            RequestTime: new Date().toISOString(),
        },
    }).promise();
}

function toUrlString(buffer) {
    return buffer.toString('base64')
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=/g, '');
}

function errorResponse(errorMessage, awsRequestId, callback) {
  callback(null, {
    statusCode: 500,
    body: JSON.stringify({
      Error: errorMessage,
      Reference: awsRequestId,
    }),
    headers: {
      'Access-Control-Allow-Origin': '*',
    },
  });
}
```
