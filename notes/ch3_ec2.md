## 3.1 Introducing EC2
* Elastic Compute Cloud (EC2)
  * Amazon Elastic Compute Cloud is a part of Amazon.com's cloud-computing platform, Amazon Web Services, that allows users to rent virtual computers on which to run their own computer applications. (Wikipedia)
  * Initial release: 2006
* Secure, resizable compute capacity in the cloud
* Like a virtual machine, but hosted in AWS instead of your own data center
* Designed to make web-scale cloud computing easier for developers.
* Provides flexible capacity and complete control over your own instances.
* __instance__ - "An instance in cloud computing is a server resource provided by third-party cloud services. While you can manage and maintain physical server resources on premises, it is costly and inefficient to do so. Cloud providers maintain hardware in their data centers and give you virtual access to compute resources in the form of an instance. You can use the cloud instance for running compute-intensive workloads like containers, databases, microservices, and virtual machines." [1]
* Positives
  * AWS was the first cloud provider when they introduced EC2 as the first virtual machine running in a public cloud.
  * led to a big change in the IT industry. Previously, companies needed to use on-premises infrastructure(physical servers in a data center). This was a long-term (3-5 year) investment usually and there was a lot of wasted capacity because they would start with a lot of compute capacity with the expectation that their app would "grow into" it. It would also take a lot of time (usually 3 month minimum for large businesses → approval for purchase, wait for availability of parts, wait for delivery, physical installation in data center, patch network cables and get the server on the right network → sys ops team installs the operating system along with any monitoring and backup and security hardening software as well → finally you can install an application on your server) to set up their physical servers.
  * EC2 changed the economics of computing. You only pay for the time your EC2 instance is up and running. (Pay as you go model).
  * No wasted capacity (grow and shrink when you need). Wasted capacity is inherent in physical servers because when first set up they are often overspecified. But with EC2 as the demands on your application change, you can grow and shrink your infrastructure to suit the requirements of our application. 

  ## 3.4 Launching an EC2 Instance - Demo
  * Key Pair (login)
    * You can use a key pair to securely connect to your instance. Ensure that you have access to the selected key pair before you launch the instance.
    * Controls who can log into your server. Anyone with the private key can log in.
  * Network settings
    * defailt is VPC (covered later; for now just know it's like your own virtual data center in the cloud that is private to you)
  [continue at 5:00]



  ## References
  [1] https://aws.amazon.com/what-is/cloud-instances/