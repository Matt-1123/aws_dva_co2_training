https://aws.amazon.com/certification/certified-developer-associate/
"AWS Certified Developer - Associate showcases knowledge and understanding of core AWS services, uses, and basic AWS architecture best practices, and proficiency in developing, deploying, and debugging cloud-based applications by using AWS. Preparing for and attaining this certification gives certified individuals more confidence and credibility. Organizations with AWS Certified developers have the assurance of having the right talent to give them a competitive advantage and ensure stakeholder and customer satisfaction."

## Ch 1: Introduction
DVA-CO2 exam
Intermediate course
Prereq: Solutions Architect Exam/Course

Exam Guide
* 130 minutes
* 65 questions (15 unscored)
* passing: 720/1000
* $150
* Multiple choice and multiple response
* All questions are **Scenario-based** questions
* Qualification valid for three years
* Exam Domains
  * Development with AWS Services: 32% weight
  * Security: 26%
  * Deployment: 24%
  * Troubleshooting and Optimization: 18%

AWS Cloud Sandbox
* allows you to provision and configure AWS resources using a live AWS account that cloud guru provides

Introducing Hands On Labs
* provide practical experience
* labs create live aWS environments you can use to complete lab objectives

Course Updates
* content is updated to line up with latest version of DVA CO2 exam. 
* Deemed current as of June 2023.

## Ch. 2: Beginner's Guide to IAM

### Glossary
__Access key__ - Access keys are long-term credentials for an IAM user or the AWS account root user. You can use access keys to sign programmatic requests to the AWS CLI or AWS API (directly or using the AWS SDK). They consist of two parts: an **access key ID** (for example, AKIAIOSFODNN7EXAMPLE) and a **secret access key** (for example, wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY). You must use both the access key ID and secret access key together to authenticate your requests.

IAM 101
* Identity and access management
* Manage users and their level of access to the AWS console.
* Gives you centralized control of your AWS account, shared access to your AWS account, granular permissions, enables identity federation (which allows users to login using Google, Facebook, Amazon, etc.), allows multi-factor authentication (MFA), temporary access as necessary (e.g. time limited access to s3 bucket or database.)
* Users, Groups, and Roles
  * __Users__: An IAM user is an identity with long-term credentials that is used to interact with AWS in an account.
  * __Groups__: a collection of users under one set of permissions
  * __Roles__: you create roles and can then assign them to users, applications, and services to give access to AWS resources; example: S3 bucket access role, given to an EC2 instance to access media files.
* __IAM Policy__: a document that defines one or more permissions; an IAM policy can be attached to a user, group, and/or role (best practice to attach to groups only)

IAM Demo
* __MFA__
  * First, it's wise to set up multi-factor authentication for the root user AWS account. 
    > Use MFA to increase the security of your AWS environment. Signing in with MFA requires an authentication code from an MFA device. Each user can have a maximum of 8 MFA devices assigned. 
    * The __root user__ has access to all AWS resources in this account
    * To set up MFA, you can use:
      * an authenticator app installed on your computer or smart phone, 
      * a security key
      * a hardware TOTP token, which shows a code displayed on a hardware time-based one-time password (TOTP) token.
* Add a new user: 
  * When adding a new user, you can add permissions by adding the user to a group, copying permissions from another user, or attaching a managed policy (however, it's recommended to only attach policies to groups).
  * For this demo, create a group for this user.
    * Title the group 'sysadmin'.
    * Provide this group the permission policy, 'AdministrationAccess', which is an AWS managed permission policy giving full access to all AWS services.
  * Add another user, and assign them to a new group called 'dev' which has read-only access to S3 (choose the AWS S3 Read Only managed policy).
* Password policy
  * You can change the minimum password length, characters needed, etc. from Account Settings
  * Allows organizations to reflect their own password policies for their AWS users.
* Create a Role
  * Step 1. Select trusted entity
    * Trusted entity types:
      * AWS Service - Allow AWS services like EC2, Lambda, or others to perform actions in this account.
      * AWS Account - Allow entities in other AWS accounts belonging to you or a 3rd party to perform actions in this account.
      * Web identity - Allows users federated by the specified external web identity provider to assume this role to perform actions in this account.
      * SAML 2.0 federation - Allow users federated with SAML 2.0 from a corporate directory to perform actions in this account.
      * Custom trust policy - Create a custom trust policy to enable others to perform actions in this account.
    * In this demo, choose an AWS Service as the trusted entity
  * Step 2. Select permissions
    * In this demo, delect AmazonS3FullAccess
  * Step 3. Create a role name, e.g. 'EC2S3FullAccess', and description, e.g. 'Allows EC2 instances to call AWS services on your behalf.'
  * IAM roles allow applications to securely make API requests from EC2 instances, without requiring you to manage the security credentials that the applications use.
Lab: 'Introduction to AWS Identity and Access Management (IAM)'
* Explicit 'Deny' overrides any explicit 'Allow'. By default, all resources are implicit 'Deny'.
* __least-privilege security__ - an IAM security best practice in which you only allow users the bare minimum permissions needed to do their job function(s).

Testing IAM Permissions
* __IAM Policy Simulator__ - test the effects of IAM policies before committing them to production to validate that the policies work as expected and don't allow or deny the wrong services.
* You can also test existing users and the policies attached to them. This is helpful in troubleshooting an issue you suspect may be IAM related.
* https://policysim.aws.amazon.com/

IAM101 - Summary
* IAM consists of users, groups, and roles
* Example IAM Policy Document:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

* You can allow or deny read, allow, update, etc actions on resources.
* IAM is universal, not regional.
  * This means anything you set up within IAM can be used within any region.
* The __root account__ is created when you first set up your AWS account. The account has complete admin access. 
  * For day to day activities, it's recommended that you use an IAM user account rather than the root account.
* New users have no permissions when first created, i.e. they start with implicit deny for all actions on all resources.
* IAM Access Keys - exam tips
  * __access key__ - new users are assigned an access key ID and a secret access key when their accounts are created.
  * These keys are not the same as a password; you cannot use them to log into the AWS management console. Rather, these allow programmatic access to AWS via the APIs and the command line.
  * You can only view the access key ID and secret access key once. If lost, you need to regenerate them. So be sure to save these in a secure location.
  * Always set up Multi-Factor Authentication (MFA) on your root account.
  * Password rotation - you can create and customize your own password rotation policies.
