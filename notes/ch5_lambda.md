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

