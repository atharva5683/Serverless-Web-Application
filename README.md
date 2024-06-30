# Serverless Web Application Workshop

# Overview

We will implement a basic online application in this session that lets customers book unicorn rides from the Wild Rydes fleet. Users can specify the place where they would want to be picked up using an HTML-based user interface provided by the application. To submit the request and send a nearby unicorn, it will communicate with a RESTful web service on the backend. Before requesting a ride, users will be able to register with the service and log in through the application.

## Prerequisites

To complete this tutorial, you will need an AWS account, AWS CLI installed, an account with ArcGIS to add mapping to your app, a text editor, and a web browser. If you don't already have an AWS account, you can follow the Setting Up Your AWS Environment getting started guide for a quick overview.

## Application architecture

The application architecture uses AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

<img width="1312" alt="wildrydes-complete-architecture" src="https://github.com/atharva5683/Serverless-Web-Application/assets/160429511/8d181ffe-1428-45c6-a123-960094121793">

## Modules
This is divided into five modules. Each module describes a scenario of what we're going to build and step-by-step directions to help you implement the architecture and verify your work. 

1) Host a Static Website : Configure AWS Amplify to host the static resources for your web application with continuous deployment built in
2) Manage Users : Create an Amazon Cognito user pool to manage your users' accounts
3) Build a Serverless Backend : Build a backend process for handling requests for your web application
4) Deploy a RESTful API : Use Amazon API Gateway to expose the Lambda function you built in the previous module as a RESTful API
5) Terminate Resources : Terminate all the resources you created throughout this tutorial
