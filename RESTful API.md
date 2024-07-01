# Deploy a RESTful API

## Overview

The Lambda function you constructed in the previous module will be exposed as a RESTful API in this module via Amazon API Gateway. The general Internet will have access to this API. The user pool you established for Amazon Cognito in the preceding module will be used to secure it. You will next add client-side JavaScript that makes AJAX requests to the available APIs using this setup, transforming your statically hosted website into a dynamic web application.


## Architecture overview

![Serverless_Web_App_LP_assets_05 d1ecdfaab160d7dc00ddbc9e0245fa34b8d8f26b](https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/bed19d2c-3915-4681-9d96-974c0819f385)

The integration between the API Gateway component you will construct in this module and the other components you have already built is depicted in the diagram above.

An existing page on the static website you published in the first module is set up to communicate with the API you will create in this module. To book a unicorn ride, visit the /ride.html page, which features a straightforward map-based interface. Your users will be able to choose their pickup location by clicking on a place on the map and then requesting a ride by clicking on the "Request Unicorn" button located in the upper right corner of the website after authenticating via the /signin.html page.


## Create and Deploy REST API 

# Step 1: Create a REST API

1. Log in to the Amazon API Gateway console.
2. In the left navigation pane, select "APIs".
3. Choose "Build" under "REST API".
4. In the "Choose the protocol" section, select "REST".
5. In the "Create new API" section, select "New API".
6. **Settings:**
   - Enter "WildRydes" for the API name.
   - Select "Edge optimized" in the "Endpoint Type" dropdown.
     *Note: Use edge-optimized endpoint types for public services being accessed from the Internet. Regional endpoints are typically used for APIs that are accessed primarily from within the same AWS Region.*
7. Choose "Create API".

---

# Step 2: Create a Cognito User Pools Authorizer

1. In the left navigation pane of the WildRydes API, select "Authorizers".
2. Choose "Create New Authorizer".
3. **Authorizer Configuration:**
   - Enter "WildRydes" into the "Authorizer Name" field.
   - Select "Cognito" as the "Type".
   - **Cognito User Pool:**
     - In the "Region" dropdown, select the same Region you have been using for the rest of the tutorial.
     - Enter "WildRydes" in the "Cognito User Pool name" field.
   - Enter "Authorization" for the "Token Source".
4. Choose "Create".
5. **Verification:**
   - Select "Test".
   - Paste the Authorization Token copied from the ride.html webpage in the "Validate your implementation" section of Module 2 into the "Authorization (header)" field.
   - Verify that the HTTP status Response code is 200.

---

# Step 3: Create a Resource and POST Method

1. In the left navigation pane of your WildRydes API, select "Resources".
2. From the "Actions" dropdown, select "Create Resource".
3. **Resource Setup:**
   - Enter "ride" as the "Resource Name", which will automatically create the "Resource Path" /ride.
   - Select the checkbox for "Enable API Gateway CORS".
4. Choose "Create Resource".
5. **Create POST Method:**
   - With the newly created /ride resource selected:
     - From the "Actions" dropdown, select "Create Method".
     - Select "POST" from the new dropdown that appears under "OPTIONS", then select the checkmark icon.
   - Select "Lambda Function" for the "Integration type".
   - Select the checkbox for "Use Lambda Proxy integration".
   - Select the same Region you have been using throughout the tutorial for "Lambda Region".
   - Enter "RequestUnicorn" for "Lambda Function".
6. Choose "Save".
   - *Note: If you get an error that your function does not exist, check that the region you selected matches the one you used in the previous modules.*
7. **Configure Authorization:**
   - Select the "Method Request" card.
   - Choose the pencil icon next to "Authorization".
   - Select the "WildRydes Cognito user pool authorizer" from the drop-down list, and select the checkmark icon.
