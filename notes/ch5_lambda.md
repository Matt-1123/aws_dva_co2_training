## 5.1 - Serverless 101
* Going serverless enables you to build scalable applications quickly without managing any servers
* What to know: 
  * No need for server management
  * Automatic continuous scaling (as needed)
  * Only pay when your code executes (including free-tier options)
    * Serverless applications are __event-driven__ and you are only charged for when your code is executed
  * Independent: each event will trigger a single function
* Examples of AWS serverless technologies: 
  * Lambda: enables you to run code as functions without provisioning any servers. 
  * SQS (Simple Queue Service) - A message queuing service that allows you to decouple and scale your applications. 
    * __decoupled application__ - "In microservice architectures, applications are built and deployed as highly decoupled, focussed services. A decoupled application architecture allows each component to perform its tasks independently - it allows components to remain completely autonomous and unaware of each other. A change in one service shouldn't require a change in the other services. It is the process of separating services so that their functionality is more self-contained."
  * SNS (Simple Notification Service) - A messaging service for sending text messages, mobile notifications, and emails.
  * API Gateway - allows you to create, publish, and secure APIs at any scale.
  * DynamoDB - fully managed NoSQl database
  * S3 - Object storage and web hosting
* Serverless website example: When a user enters the address, their browser sends a request to the API Gateway and your request is then routed to the appropriate Lambda function. Each lambda function handles different functionalities of the website, e.g. discussion forums, playing videos (from S3). It can also use 3P services, such as credit card payments, other non-AWS databases, etc.

## 5.2 - Introducing Lambda
* __Serverless Compute__
  * Run your code in AWS without provisioning any servers.
  * Lambda takes care of everything required to run your code, including the runtime environment.
* Supported languages: Node.js, Python, Java, C#, Ruby, PowerShell, and Go
* Pricing is based on the number of requests, their duration (milliseconds), and the amount of memory (GB-seconds) used by your Lambda function. 
* Event-driven architecture:
  * event-driven - Lambda functions can be automatically triggered by other AWS services, or called directly from any web or mobile app.
  * triggered by events - these events could be things like changes to data in an S3 bucket, or a call from the Alexa cloud.
  * triggered by user requests - you can use API Gateway to configure an HTTP endpoint, allowing you to trigger your function at any time using an HTTP request.

## 5.3 Lab - Creating a Lambda Function Using the AWS Console
Learning objectives
1. Author Node.js Lambda Function in the AWS Console. 
  * Create your Lambda function in the AWS Management Console using the code provided in the GitHub repo for the course. Set the Node.js runtime version to 20.x.
2. Create a Test Event and Execute Lambda
  * Create a basic hello-world test event to test the Lambda function. This test will invoke the function and determine the HTTP response code for the website in the code.
3. Verify That CloudWatch Has Captured Function Logs
  * After you've tested the function, navigate to CloudWatch to review the logs for the test event.

Lambda Permissions
* __execution role__: "Choose a role that defines the permissions of your function. To create a custom role, go to the IAM console" ... "Lambda will create an execution role named '[lambda name]-role-[random string]', with permission to upload logs to Amazon CloudWatch Logs."
* __lambda test event__: "A test event is a JSON object that mocks the structure of requests emitted by AWS services to invoke a Lambda function. Use it to see the function’s invocation result."
* __Lambda CloudWatch metrics__: "Lambda sends runtime metrics for your functions to Amazon CloudWatch. The metrics shown are an aggregate view of all function runtime activity. To view metrics for the unqualified or $LATEST resource, choose Filter by. To view metrics for a specific function version or alias, choose Aliases or Versions, select the alias or version, and then choose Monitor." 
* __Lambda CloudWatch logs__: "Lambda sends runtime metrics for your functions to Amazon CloudWatch. The metrics shown are an aggregate view of all function runtime activity. To view metrics for the unqualified or $LATEST resource, choose Filter by. To view metrics for a specific function version or alias, choose Aliases or Versions, select the alias or version, and then choose Monitor."
  * Monitor > View CloudWatch Logs (This will open a new tab, taking you to the CloudWatch Managment Console.)
  * You should see the START and END Request Id, as well as the Billed Duration.
  * My logs:
    `START RequestId: 4695d490-e342-4c23-97d1-3fba2ce875a3 Version: $LATEST`
    `4695d490-e342-4c23-97d1-3fba2ce875a3	INFO	status code: 200`
    `END RequestId: 4695d490-e342-4c23-97d1-3fba2ce875a3`
    `REPORT RequestId: 4695d490-e342-4c23-97d1-3fba2ce875a3	Duration: 726.71 ms	Billed Duration: 727 ms	Memory Size: 128 MB	Max Memory Used: 70 MB	Init Duration: 156.45 ms	`
* Function code:
  `holURLChecker/index.js`
  ```
  const https = require('https');

  // let url = "https://www.fjklsa532;jflksdjkl.com";
  let url = "https://www.amazon.com";

  exports.handler = async function(event) {
      let statusCode;
      console.log()
      await new Promise(function(resolve, reject) {
          https.get(url, (res) => {
              statusCode = res.statusCode;
              resolve(statusCode);
          }).on("error", (e) => {
              reject(Error(e));
          });
      });
      console.log(`status code: ${statusCode}`);
      return statusCode;
  };
  ```

