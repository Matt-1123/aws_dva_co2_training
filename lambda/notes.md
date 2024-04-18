## AWS lambda tutorial
* Source: https://serverlessland.com/getting-started/lambda/#step1
>Your Lambda function interacts with DynamoDB using the AWS SDK for NodeJS. The AWS SDK is already available to your Lambda function code as a modular package dependency. You must to require each package in your function code.
>Use the PUT action from the DynamoDB SDK to persist a new item to the table. The item will contain data from the submitted form. This data is appended to the function URL as query string parameters and available to your function code from the event.queryStringParameters object.