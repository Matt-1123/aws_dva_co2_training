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
IAM 101
* Identity and access management
* Manage users and their level of access to the AWS console.
* Gives you centralized control of your AWS account, shared access to your AWS account, granular permissions, enables identity federation (which allows users to login using Google, Facebook, Amazon, etc.), allows multi-factor authentication (MFA), temporary access as necessary (e.g. time limited access to s3 bucket or database.)
* Users, Groups, and Roles
  * __Users__: An IAM user is an identity with long-term credentials that is used to interact with AWS in an account.
  * __Groups__: a collection of users under one set of permissions
  * __Roles__: you create roles and can then assign them to users, applications, and services to give access to AWS resources
* __IAM Policy__: a document that defines one or more permissions; an IAM policy can be attached to a user, group, and/or role

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

[continue at 9:57]