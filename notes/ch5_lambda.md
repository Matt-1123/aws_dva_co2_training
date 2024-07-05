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
  * triggered by user requests - you can use the API Gateway to configure an HTTP endpoint, allowing you to trigger your function at any time using an HTTP request.

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

## 5.4 - API Gateway
* API Gateway is a service which allows you to publish, maintain, and monitor APIs at any scale
* It provides a single endpoint for all client traffic interacting with the backend of your application.
* How it works: a user connects with a client device, e.g. web broswer. They make a request to the AWS environment, and the request hits the API Gateway. API Gateway can then forward the request to different services, such as Lambda.
* What is it? API Gateway allows you to connect to applications running on Lambda, EC2, or Elastic Beanstalk and services like DynamoDB and Kinesis.
* Supports multiple endpoints and targets, allows you to send each API endpoint to a different target.
* Supports multiple versions, e.g. you can have a different version of your API for production, testing, and development.
* An API is like a front door for applications to access data, business logic, or functionality from your backend services, e.g. applications running on Lambda or EC2.
* Supported API types: 
  * RESTful APIs: optimized for stateless, serverless workloads
  * Websocket APIs: for real-time, two-way, stateful communication, e.g. chat apps
* __RESTful API__
  * API Gateway supports both types of RESTful APIs: HTTP APIs and REST APIs.
  * REST = Representational State Transfer
  * Optimized for serverless and web applications
  * Stateless (nothing is persisted in the application or API itself)
  * Supports JSON (JavaScript Object Notation), a notation language that uses key-value pairs.
* Serverless
* Integrates with CloudWatch, so it logs all API calls, latencies, and error rates.
* Supports throttling: helps you manage traffic with throttling so that backend operations can withstand traffic spikes and denial of service attacks.
  * "A denial-of-service (DoS) attack is a type of cyber attack in which a malicious actor aims to render a computer or other device unavailable to its intended users by interrupting the device's normal functioning."
* Alias vs. version


## 5.5 - Building a Serverless Website - Demo
* With API Gateway, you can add an API to your Lambda function to create an HTTP endpoint that invokes your function. 
* Lambda => Add trigger => API Gateway
* You can optionally configure security mechanisms (with JWT Authorizer) for your API endpoint, or just leave it open.
* To allow the webpage to successfully call the API, under Lambda => Add trigger => API Gateway => Additional Settings, check 'Cross-origin resource sharing (CORS)'. This is needed when your API isn't hosted on the same domain as your webpage. This option enables cross-origin resource sharing from any domain by adding the `Access-Control-Allow-Origin` header to all responses.
* Add an S3 bucket
  * Object ownership: Control ownership of objects written to this bucket from other AWS accounts and the use of access control lists (ACLs). Object ownership determines who can specify access to objects. Options:
    1. ACLs disabled (recommended) - all objects in this bucket are owned by this account. Access to this bucket and its objects is specified using only policies. i.e. not suitable for public webpages.
    2. ACLs enabled - Objects in this bucket can be owned by other AWS accounts. Access to this bucket and its objects can be specified using ACLs (access control lists). 
  * In the bucket's Properties, enable static website hosting
    * docs: https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html?icmpid=docs_amazons3_console
* Note on Static website hosting with S3, from docs:
  > You can use the AWS Amplify Console to host a single-page web app. The AWS Amplify Console supports single-page apps built with single-page app frameworks (for example, React JS, Vue JS, Angular JS, and Nuxt) and static site generators (for example, Gatsby JS, React-static, Jekyll, and Hugo). For more information, see Getting Started in the AWS Amplify Console User Guide.
  >
  >Amazon S3 website endpoints do not support HTTPS. If you want to use HTTPS, you can use Amazon CloudFront to serve a static website hosted on Amazon S3. For more information, see How do I use CloudFront to serve HTTPS requests for my Amazon S3 bucket?
  >To use HTTPS with a custom domain, see Configuring a static website using a custom domain registered with Route 53.
* Lambda function aliases:
  * "You can create aliases for your Lambda function. A Lambda alias is a pointer to a function version that you can update. The function's users can access the function version using the alias Amazon Resource Name (ARN). When you deploy a new version, you can update the alias to use the new version, or split traffic between two versions." https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html
* An __unqualified function ARN__ (no version specified at the end of the ARN string) defaults to __$LATEST__. 
* ** point aliases to versions **
* Lambda assigns the function URL to the $LATEST unpublished version of your function. You cannot assign a function URL to any other function version, but you can assign a function URL to any function alias.
* Each version, including $LATEST, has its own function URL. 
* Exam tips
  * $LATEST is always the last version of code you uploaded to Lambda
  * Versioning and aliases: Use Lambda versioning and aliases to point your applications to a specific version if you don't want to use $LATEST.
  * Example ARNs:
    * arn:aws:lambda:us-east-1:123456789012:function:mylambda:Prod
    * arn:aws:lambda:us-east-1:123456789012:function:mylambda:$LATEST
    * arn:aws:lambda:us-east-1:123456789012:function:mylambdA (defaults to $LATEST)

## 5.7 Lambda Concurrent Executions Limit
* Exam tips
  * 1000 concurrent executions per second (default limit)
  * Hitting the limit results in a 429 HTTP status code
  * The remedy is to get the limit raised by AWS Support
  * Reserved concurrency guarantees a set number of concurrent execututions are always available to a critical function.
* Don't need to memorize specifics
* Just need to be aware that there is a concurrent execution limit for Lambda
* This is a safety feature to limit the number of concurrent executions across all functions in a given region per account.
* Default is 1000 concurrent executions per region. 
* Surpassing the limit results in a `TooManyRequestsException` error and HTTP status code of 429 (Request throughput limit exceeded)
* You can request an increase by contacting AWS support.
  * For example A Cloud Guru has a 6.5 million invocations per day limit for their one region, us-east-1
* __Reserved concurrency__ guarantees that a set number of executions which will always be available for your critical function. However, this also acts as a limit (e.g. setting a reserved concurrency of 500 means that function can never surpass 500 concurrent executions)

## 5.8 Lambda and VPC Access
* Enabling Lambda to access your VPC Resources
  * Some use cases require Lambda to access resources which are inside a private VPC
  * __Amazon Virtual Private Cloud (VPC)__ is a service that lets you launch AWS resources in a logically isolated virtual network that you define.
  * e.g. read or write to an RDS database, or shut down an EC2 instance in response to a security alert.
* To enable Lambda to access VPC resources, you need to allow the function to connect to the private subnet.
* Lambda requires the following VPC Configuration information to connect to the VPC:
  * Private subnet ID
  * Security group ID (with required access)
  * Lambda uses this info to set up ENIs using an available IP address from your private subnet
* You add VPC information to your Lambda function config using the vpc-config parameter, e.g.:
  `aws lambda update-function-configuration --function-name my-function --vpc-config SubnetIds=subnet-1122aabb,SecurityGroupIds=sg-51530134`
* Exam tips
  * It is possible to enable Lambda to access resources which are inside a private VPC.
  * Provide VPC config to the function - private subnet ID, security group ID
  * Lambda uses the VPC information to set up ENIs using an IP from the private subnet CIDR range.
  * The security group then allows your function to access resources in VPC.

## 5.9 Example Serverless Architectures
* Characteristics of Event-Driven Architecture
  1. Asynchronous - events and asynchronous communication are used to loosely couple application components. An event or message might trigger an action, but no response is expected or required for the next step to occur.
  2. Loosely coupled - services and components operate and scale independently of each other.
  3. Single-purpose functions - stateless functions performing a short-lived task
* __Event source__ - e.g. object uploaded to S3, item in a table modified
* __Event destination__
* __Event Router__ - e.g. __EventBridge__ AWS service; coordinates everything between the event source and destination, e.g. if an event does more than just trigger a single Lambda function, it may be complex enough to need an event router.
  * Example: Image processing application
    * 'UploadedImages' S3 bucket -> Image Processing Lambda function -> 'ProcessedImages' S3 bucket AND DynamoDB table for image metadata <- Processed images served by CloudFront
* Exam tips: 
  * Event driven and asynchronous: an event or message may trigger an action, but no response is expected or required
  * Building blocks - think of AWS services as building blocks that can be integrated together to create an application
  * Loosely coupled - SQS allows you to queue messages. EventBridge helps you handle events and route them to application components.
  * Flexibility and Scalability - services and components operate and scale independently of each other.

## 5.10 Step Functions - Demo
* __Step Functions__ allow you to visualize and test your serverless applications. Step Functions provide a graphical console to arrange and visualize the components of your application as a series of steps. This makes it simple to build and run multistep applications. Step Functions automatically trigger and track each step, and retries when there are errors, so your application executes in order and as expected. Step Functions log the state of each step, so when things do go wrong, you can diagnose and debug problems quickly
* __Sequential Steps__, e.g.  
  (Start) → [Upload RAW File] → [Delete Raw File] → (End)
* __Branching Steps__ (choice of path), e.g.
* __Parallel steps__
* Great way to visualize your serverless application

## 5.11 Building a Serverless Application Using Step Functions, API Gateway, Lambda, and S3 in AWS
* Amazon SES > Configuration: Identities > Create Identity
    >An identity is a domain, subdomain, or email address you use to send email through Amazon SES. Identity verification at the domain level extends to all email addresses under one verified domain identity.
* [optional - not used in lab] __Custom MAIL FROM domain__: Messages sent through Amazon SES will be marked as originating from your domain instead of a subdomain of amazon.com.
  ### Results of Lab
  * Attempt 4/29/24: completed all staps, but the static website url hosted on S3 failed to load, with the error, "Yikes! There was an error: TypeError: Failed to fetch"

* Instead of Cloud Playground, I will be attempting this lab in my own account. I'll also be using Node JS instead of Python.

Notes
* Lambda handlers:
  ```
  export const handler = async (event, context) => {
    console.log("EVENT: \n" + JSON.stringify(event, null, 2));
    return context.logStreamName;
  };
  ```
  >When you configure a function, the value of the handler setting is the file name and the name of the exported handler method, separated by a dot. The default in the console and for examples in this guide is index.handler. This indicates the handler method that's exported from the index.js file.
  >The runtime passes arguments to the handler method. The first argument is the event object, which contains information from the invoker. The invoker passes this information as a JSON-formatted string when it calls Invoke, and the runtime converts it to an object. When an AWS service invokes your function, the event structure varies by service.
  >The second argument is the context object, which contains information about the invocation, function, and execution environment. In the preceding example, the function gets the name of the log stream from the context object and returns it to the invoker.
  >You can also use a callback argument, which is a function that you can call in non-async handlers to send a response. We recommend that you use async/await instead of callbacks. Async/await provides improved readability, error handling, and efficiency.
  * More on callbacks in Lambda: https://docs.aws.amazon.com/lambda/latest/dg/nodejs-handler.html#nodejs-handler-callback
* __Node.js event loop__ - 
  >When you use callbacks in your handler, the function continues to execute until the event loop is empty or the function times out. The response isn't sent to the invoker until all event loop tasks are finished. If the function times out, an error is returned instead. You can configure the runtime to send the response immediately by setting context.callbackWaitsForEmptyEventLoop to false.
    * https://docs.aws.amazon.com/lambda/latest/dg/nodejs-handler.html#nodejs-handler-callback
  >The event loop is what allows Node.js to perform non-blocking I/O operations — despite the fact that JavaScript is single-threaded — by offloading operations to the system kernel whenever possible.   
    * https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick

* Difference between require and import in JS:
  * CommonJS uses require and module.exports
  * ES6 uses import and export
  * You can't selectively load only the pieces you need with require but with import, you can selectively load only the pieces you need, which can save memory.
    e.g. `import {a} from 'b'`

* Loading is synchronous(step by step) for require on the other hand import can be asynchronous(without waiting for previous import) so it can perform a little better than require.

* S3 Permissions: Access Control List (ACL)
  >A majority of modern use cases in Amazon S3 no longer require the use of ACLs. We recommend that you keep ACLs disabled, except in unusual circumstances where you need to control access for each object individually.
    * https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html#object-acl

* REST vs HTTP vs WebSocket APIs: 
  * "REST APIs support more features than HTTP APIs, while HTTP APIs are designed with minimal features so that they can be offered at a lower price. Choose REST APIs if you need features such as API keys, per-client throttling, request validation, AWS WAF integration, or private API endpoints."
  * "An HTTP API is based on traditional web principles and uses the HTTP protocol for communication. A REST API, on the other hand, is based on Representational State Transfer principles and uses web resources for communication. As a result, REST APIs are more flexible and scalable than HTTP APIs."
  * "WebSocket APIs maintain persistent connections with clients for full-duplex communication"
    * "A duplex communication system is a point-to-point system composed of two or more connected parties or devices that can communicate with one another in both directions." (wikipedia)

* How API Gateway and Lambda communicate:
  * "The HTTP API provides an HTTP endpoint for your Lambda function. API Gateway routes requests to your Lambda function, and then returns the function's response to clients."
