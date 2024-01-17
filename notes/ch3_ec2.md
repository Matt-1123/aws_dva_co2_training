## 3.1 Introducing EC2
* Elastic Compute Cloud (EC2)
  * Amazon Elastic Compute Cloud is a part of Amazon.com's cloud-computing platform, Amazon Web Services, that allows users to rent virtual computers on which to run their own computer applications. (Wikipedia)
  * Initial release: 2006
* Secure, resizable compute capacity in the cloud
* Like a virtual machine, but hosted in AWS instead of your own data center
* Designed to make web-scale cloud computing easier for developers.
* Provides flexible capacity and complete control over your own instances.
* __instance__ - "An instance in cloud computing is a *server resource* provided by third-party cloud services. While you can manage and maintain physical server resources on premises, it is costly and inefficient to do so. Cloud providers maintain hardware in their data centers and give you *virtual access to compute resources* in the form of an instance. You can use the cloud instance for running compute-intensive workloads like containers, databases, microservices, and virtual machines." [1]
* Positives
  * AWS was the first cloud provider when they introduced EC2 as the first virtual machine running in a public cloud.
  * led to a big change in the IT industry. Previously, companies needed to use on-premises infrastructure(physical servers in a data center). This was a long-term (3-5 year) investment usually and there was a lot of wasted capacity because they would start with a lot of compute capacity with the expectation that their app would "grow into" it. It would also take a lot of time (usually 3 month minimum for large businesses → approval for purchase, wait for availability of parts, wait for delivery, physical installation in data center, patch network cables and get the server on the right network → sys ops team installs the operating system along with any monitoring and backup and security hardening software as well → finally you can install an application on your server) to set up their physical servers.
  * EC2 changed the economics of computing. You only pay for the time your EC2 instance is up and running. (*Pay as you go model*).
  * No wasted capacity (grow and shrink when you need). Wasted capacity is inherent in physical servers because when first set up they are often overspecified. But with EC2 as the demands on your application change, you can grow and shrink your infrastructure to suit the requirements of our application. 

## 3.4 Launching an EC2 Instance - Demo
* __Key Pair__ (login) - You can use a key pair to securely connect to your instance. Ensure that you have access to the selected key pair before you launch the instance.
  * You can use a key pair to securely connect to your instance. Ensure that you have access to the selected key pair before you launch the instance.
  * Controls who can log into your server. Anyone with the private key can log in.
  * When prompted, store the private key in a secure and accessible location on your computer. You will need it later to connect to your instance. 
* Network settings
  * defailt is __VPC__ (covered later; for now just know it's like your own virtual data center in the cloud that is private to you)
[continue at 5:00]

## 3.5 
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html
* Use Putty to SSH into an EC2 instance from a Windows machine.
* __.pem file__ - the file containing the key pair that's generated when you first create an EC2 instance. Generated in a .pem format.
* Putty, however, uses a format called .ppk, so we'll need to convert the .pem file to .ppk format.
* Once converted, I can connect to the EC2 instance using the public IP address.
* Network settings
  * A __security group__ is a set of firewall rules that control the traffic for your instance. Add rules to allow specific traffic to reach your instance.
    * __firewall__ - "In computing, a firewall is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules. A firewall typically establishes a barrier between a trusted network and an untrusted network, such as the Internet" (Wikipedia)
  * Rules with source of 0.0.0.0/0 allow all IP addresses to access your instance. We recommend setting security group rules to allow access from known IP addresses only.
* t2.micro free tier: "Free tier: In your first year includes 750 hours of t2.micro (or t3.micro in the Regions in which t2.micro is unavailable) instance usage on free tier AMIs per month, 30 GiB of EBS storage, 2 million IOs, 1 GB of snapshots, and 100 GB of bandwidth to the internet."
* EC2 instance public IPv4 address: 52.72.141.154
* In Putty, for Host Name, input `ec2-user@52.72.141.154`
* In Putty, go to `Connection > SSH > Auth > Credentials` and for 'Private key file for authentication', select the downloaded .ppk file containing the private key we've just generated from the EC2 instance.
* To avoid having to do all this config again in Putty, save `ec2-user@52.72.141.154` as a Saved Session. Next time, just have to select this session and hit Load.
* Select 'Open' and a terminal opens up. We're now connected to our EC2 instance.

```
Using username "ec2-user".
Authenticating with public key "putty-demo" from agent
  ,     #_
  ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
  ~~       V~' '->
    ~~~         /
      ~~._.   _/
        _/ _/
      _/m/'
[ec2-user@ip-172-31-30-160 ~]$

```

* To update the OS, run:
```
sudo su
yum update -y
```





## References
[1] https://aws.amazon.com/what-is/cloud-instances/