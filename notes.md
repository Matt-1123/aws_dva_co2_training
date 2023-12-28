## Ch 1: Introduction
DVA-CO2 exam
Intermediate course
Prereq: Solutions Architect Exam/Course
Skip (for now):
* Section 2 - IAM

Exam Guide
* 130 minutes
* 65 questions (15 unscored)
* passing: 720/1000
* $150
* Multiple choice and multiple response
* All questions are __Scenario-based__ questions
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
  * __Users__: end users - think people
  * __Groups__: a collection of users under one set of permissions
  * __Roles__: you create roles and can then assign them to users, applications, and services to give access to AWS resources
* __IAM Policy__: a document that defines one or more permissions; an IAM policy can be attached to a user, group, and/or role

IAM Demo