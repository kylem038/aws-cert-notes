# AWS Technical Essentials

## Module 1: Intro

### What is AWS?
There are a variety of deployment options out there including:

1. On-premises
The traditional way of doing deployments. Companies hosted and mainted hardware such as compute, storage and networking equipment in their own buildings and data centers. A demand to move these responsibilities outside the company's physical spaces was solved using cloud computing. 

2. Cloud
The on-demand delivery of IT services over the Internet using pay-as-you-go pricing. No longer need to pay for large warehouses and hardware to manage everything in-house. 

3. Hybrid
Combination between on-premise resources and cloud resources for handling deployment. An example is setting up a QA environment separate from production. This could take a lot of time and resources to setup in-premise, but using AWS it can be done in minutes. 

AWS fits in by removing the "undifferentiated heavy lifting" of certain deployment tasks. Advantages include:
- Pay as you go
- Benefit from massive economies of scale
- Stop guessing capacity
- Increase speed and agility
- Realize cost savings
- Go global in minutes

### AWS Global infrastructure
Regions, Availability Zones, and data centers exist in a redundant, nested sort of way. There are data centers inside of Availability Zones and Availability Zones inside of Regions. And how do you choose a Region? By looking at compliance, latency, pricing, and service availability. 

There are also global edge networks. Edge locations and regional edge caches are used to cache content closer to end users, thus reducing latency. You can use services like Amazon CloudFront to cache content using the edge locations.

A well-known best practice for cloud architecture is to use Region-scoped, managed services. These services come with availability and resiliency built in. When that is not possible, make sure your workload is replicated across multiple Availability Zones. At a minimum, you should use two Availability Zones.

### Interacting with AWS
Three main ways to interact with AWS are the Management Console, CLI or SDK. 

The management console is a good starting place but doesn't offer the ability for scripting or doing things programmatically. The CLI is good for that. 

Example: You want to run a report to collect data from all the servers. You need to do this programmatically every day because the server details might change. Instead of manually logging in to the console and then copying and pasting information, you can schedule an AWS CLI script with an API call to pull this data for you.

Developers commonly use AWS SDKs to integrate their application source code with AWS services. For example, consider an application with a frontend that runs in Python. Every time the application receives a photo, it uploads the file to a storage service. This action can be achieved in the source code by using the AWS SDK for Python (Boto3).

### Security and the Shared Responsbility Model
AWS covers the security "of" the cloud. We're responsible for security "in" the cloud. 

We're responible for things like IAM, updating OS and firewalls, encryption of client-side data, server-side encyption and networking traffic protection. Some examples things the "customer" is responsible for includes:
- Choosing a Region for AWS resources in accordance with data sovereignty regulations
- Implementing data-protection mechanisms, such as encryption and scheduled backups
- Using access control to limit who can access your data and AWS resources

AWS handles:
- Protecting and securing AWS Regions, Availability Zones, and data centers, down to the physical security of the buildings
- Managing the hardware, software, and networking components that run AWS services, such as the physical servers, host operating systems, virtualization layers, and AWS networking components

### Protecting AWS Root User
Root user can do anything. Don't want anyone to have access to that. Enable MFA for root users. Do not use root user for everyday tasks. Should only be used for very specific tasks. 

There are the "access keys" and "secret keys" which can be used to log in via the CLI. If you don't need the access keys for your root user don't create one unless you have to. 

Best practices for securing root user include:
- Choose a strong password
- Enable MFA
- Don't share your creds
- Disable or delete access keys for the root user
- Create IAM access for everyday tasks. 

Supported apps for using virtual MFA include Google Authenticator, Microsoft Authenticator, etc. 

### AWS IAM
Manages who can access certain accounts and availability zones, as well as read and write permissions. 

Authentication: I am who I say I am
Authorization: I can or cannot access something

IAM features includes:
- Global
- Integrated with AWS services
- Shared access
- MFA
- Identity federation (temp access to accounts based on org or company)
- Free to use

Should be as granular as you can with who gets what permissions. This can be done with policies. Policy examples can be found in the docs. 

Use IAM groups when possible and assign users to those groups, rather than on a user-by-user basis. 

*IAM roles* is an identity that can be used for temp access. Have the creds needed to sign requests, but do not use username or password. These are rotated so the ability to interact in AWS expires. 

Can be integrated with identification providers (companies that manage access to something like a company laptop). You can use this data to assign IAM roles without having to manually set it up in IAM. 

Best practices include:
- Lock down the AWS root user
- Follow principle of least privilege
- Use IAM roles when possible
- Consider using an identity provider
- Regularly review and remove unused users, roles and creds

### Demo of IAM
1. Start by creating a role. 
2. Select the entity access
3. Create policy
4. Restrict policy access
5. Add users (for admin allow for console access)
6. Create a group and attach policy to the group
7. Add user to group
8. Create access keys


## Module 2: AWS Compute

### Compute as a Service
Three fundamental compute options are VMs, container services and serverless. 

### Getting started
Amazon Machine Image is the start for EC2. You will choose what OS to run and what applications to preinstall on that instance. Other options are launch permissions and block device mapping. Default is Linux. 

The instance size and type can be changed on the fly. You should right size the resource so you don't over provision. 

When creating an EC2 you must define:
- Hardware specifications: CPU, memory, network, and storage
- Logical configurations: Networking location, firewall rules, authentication, and the operating system of your choice

AMI is the "recipe" used to create the EC2 instance. 

Where to find AMIs includes:
- Quick Start AMIs
- AWS Marketplace AMIs
- My AMIs
- Community AMIs
- Custom Image

An instance family such as "c5n.xlarge" contains the instance family (c) for compute, generation (5), attributes (n) for NVMe storage and the size (xlarge). 

Families include:
- General purpose: balanced and good for web servers or code repos
- Compute optimized: cpu bound apps like batch processing, media transcoding, scientific modeling, gaming services, ML, etc. 
- Memory optimized: memory bound apps like finance, speech recognition, fluid dynamics, etc. 
- Accelerated computing: hardware acceleration and co-processors for things that require graphics computation, floating point calculations & pattern matching
- Storage optimized: optimized for high read / write access to large data sets. Good for data warehousing, Elasticsearch & in-memory dbs 
- HPC optimimzed: high performance computing good for large simulation workloads. 

### EC2 Instance lifecycle
1. Pending
2. Running (start getting charged)
3. Reboot
4. Stopping
5. Stopped
6. Stop-Hibernate (like locking yoru computer)
7. Shutting-down
8. Terminated (gets rid of it forever) (better back things up before doing this)

Usually you want to replace an instance before terminating one. 

Pricing includes:
- On-Demand instances (no up front cost - only charged in running / stopping state)
- Spot Instances (flexible start and end times) useful for apps that are only feasible at low compute prices or with users with fault-tolerant / stateless workloads. You set the limit for how much to pay per instance hour.
- Saving plans are low usage prices for 1-3 year commitments. Good for steady usage, different instance types across different locations & those that have the money for the 1-3 year agreement. 
- Reserved instances saves up to 75% compared to on-demand. Can pay upfront, partial upfront or no upfront for 1-3 years. Good for steady-state usage, or instances that will run at certain time windows. 
- Dedicated hosts: good for compliance and hosting software licenses. 

### Demo
Launch EC2 for launching a webapp
1. Launch instances
2. Select AMI
3. Select instance type (ex. t2.xlarge)
4. Create key pair if you want to ssh in
5. Network setting (select VPC) and auto-assign public IP
    a. Select security group and the type (HTTP for public access)
    b. Add another security group for HTTPS
6. Advanced details
    a. Select IAM instance profile
    b. For user data paste in user data script for deploying the instances
    c. "stress" is a module for stress testing the app
    d. Script should go all the way to running the app
7. Run the instance and wait for it to get into the Running state. 
8. Copy the public IPv4 address to see the app running. 

User script
```
#!/bin/bash -ex
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install python3 mysql
pip3 install -r requirements.txt
amazon-linux-extras install epel
yum -y install stress
export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
export AWS_DEFAULT_REGION=<INSERT REGION HERE>
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
```

### Container Services
The second item in the compute fleet (VMs, containers and serverless)
Things like Docker or container like packages
ECS (Elastic Container Service)
EKS (Elastic K8s Service)
These are orchestrastion tools for managing a lot of different containers
Containers share the same OS and kernel as the host they exist on. VMs contain their own OS for each VM. 
*Containers can provide speed, but virtual machines offer the full strength of an operating system and more resources, like package installation, dedicated kernel, and more.*

On EC2 on instance can run many containers. ECS and EKS are there to help manage all the containers. An EC2 being managed by a container agent is called a container instance. Tasks that can be run but these agents includes:
- Launching and stopping containers
- Getting cluster state
- Scaling in and out
- Scheduling the placement of containers across your cluster
- Assigning permissions
- Meeting availability requirements

In ECS you'd create task runners which is a simple JSON file to configure the task. 

For EKS a few things differ:
1. ECS runs on instances, EKS runs on nodes
2. ECS has tasks. EKS has pods. 
3. ECS runs on AWS, EKS runs on K8s. 

### Intro to serverless
Handles undifferiented heavy lifting
Cannot access the underlying infra
For an EC2 you'd have to run patches to update dependencies and security updates. With Serverless you wouldn't have to do that. 
EC2 gives you a lot of control - serverless gives you convenience. 

### AWS Fargate
Container gets pushed to ECR
Define memory and compute resources
ECS uses Fargate with serverless options so you're not managing the EC2 instances yourself. 
"Fargate abstracts the EC2 instance so that you’re not required to manage the underlying compute infrastructure."

### Serverless with AWS Lambda
Lambda functions only runs when an event is triggered
Common triggers are HTTP requests, photo uploads for S3, events from a webapp, etc. 
Max time to run a lambda is 15 mins. Only meant for short processes. 
You want to delegate permissions to a lambda function via IAM. 
Conifuring the trigger source includes other AWS services. 

AWS SAM (Serverless Application Model)

Lambda have the following:
- Function: invoke this function when...
- Triggers: when to invoke the function
- Events: JSON containing the data for the function to process. 
- App Environment: isolated runtime for the function. 
- Deployment package: .zip or container image
- Runtime: language used to run the application. 
- Function handler: method used in function code to process events. 

### Choosing the right compute service
Use case: Shop has a spreadsheet that adds new inventory. Need to add that to the database when new inventory is added. Inventory is only added once per quarter. Using *Lambda* would be best so that when a new inventory is added a task can parse the spreadsheet for the new item and add that to the DB

Use case: App hosted on-prem needs to be pulled into AWS. It's on Linux and no refactoring should be needed. EC2 can use Linux AMI to achieve this. Containers would require refactoring.  

Use case: New app is needed using microservices design. Lower risk when changes are made to prod. Container service ECS or EKS. Containers are good for microservice designs. 

## Module 3: Networking

### Intro to Networking
Configuring the network (or VPC) allows Internet traffic to flow through the app
Using the default VPC can be dangerous from a security perspective. 
With networking the idea of sending a letter in the mail is much like a routing. 
The computer equivilent is it's IP address. It's a 32-bit address in binary. 
IPv4 notation is the decimal format converted into 8 bits separated by a period. 
Example: 192.168.1.30
CIDR notation is how you express a range of IPs. 
Example: 192.168.1.30/24
The "/24" is flexible. The higher the number after the /, the smaller number of IP addresses in the network. /28 would provide 16 IP addresses. /16 would provide 65,536 IP addresses. 

### Amazon VPC
Creating a VPC needs a Region and a IP range. 
Demo:
Check region to match what region your other resources are in. 
Then create VPC. 
Input CIDR block. 

Divide the space inside the VPC into subnets. Allows for granular control of the resources. 
An EC2 that needs internet access would be one (public) subnet, and your DB would be another (private) subnet. 
The CIDR needs to be a subset of the overall VPC CIDR. 

To enable internet connectivity you need a "Internet Gateway". 
Attach the gateway to the VPC using the Actions. 

To enable connectivity between private resources you can use a Virtual Private Gateway. 

To handle availability you should duplicate the subnets in another availability zone. 4 total subnets. 

A common starting place for those who are new to the cloud is to create a VPC with an IP range of /16 and create subnets with an IP range of /24.

To link a connection between an on-prem data center and an AWS VPC you can use AWS Direct Connect. This allows for virtual interfaces directly to public AWS services or other VPCs. 

### VPC Routing
How do requests from users know which subnet to use? This is where route tables come into play. 
Route tables exist within the Virtual private cloud. 
By default you get a local target of 10.1.0.0/16
The route table will define whether traffic can get to a public or private subnet
For a public subnet you'd need a custom route table. Create a table, edit the routes, use 0.0.0.0/0 and associate it with the internet gateway. 
Then edit subnet association and only select the 2 public subnets. 
The private VPCs will not get a route table and will use the overall VPC table, which allows for local traffic only. 

- You cannot delete the main route table.
- You cannot set a gateway route table as the main route table.
- You can replace the main route table with a custom subnet route table.
- You can add, remove, and modify routes in the main route table.
- You can explicitly associate a subnet with the main route table, even if it's already implicitly associated.

### VPC Security
2 options to control access you can use network ACLs and security groups. 
You can use ACLs to only allow for HTTPs traffic and deny everything else. 
You also need to allow for HTTPs to be inbound and outbound to allow for full functionality. 
Every EC2 would need a security group. 
The security group will also need HTTP and HTTPs allowed. 
Basic starting config would be to leave ACLs as default and control traffic in the security groups. 

Security groups block all inbound traffic but allow all outbound traffic by default. 

ACLS secure subnets only. Security groups secure instances.

By default an ACL will allow all inbound and outbound traffic. 

### Demo
1. Create VPC
2. Set CIDR range using /16
3. Create subnets
4. Select VPC
5. Create public subnet 
    a. Use /24 for the IP
6. Create private subnet
    a. Use /24 
7. Duplicate the subnets for availability. 
    a. Use a different availability zone - same region but use something like 2b instead of 2a (uswest-2b)
    b. Use non-overlapping CIDR ranges (10.1.1.0/24, 10.1.2.0/24, 10.1.3.0/24, 10.1.4.0/24)
8. Create Internet gateway
9. Attach the gateway to VPC. 
10. Create route tables. 
11. Add route to allow traffic from internet to internet gateway. 
12. Associate subnet with route table
13. Relaunch the EC2 instance (launch more like this option) to associate the EC2 with the updated VPC settings.
    a. Under network setting select the new vpc and subnet 

## Module 4: Storage
