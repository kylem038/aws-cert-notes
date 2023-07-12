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

