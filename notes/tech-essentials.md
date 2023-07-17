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

An instance family such as "c5n.xlarge" contains the instance family (c) for compute, generation (5), attributes (n) for network optimized and the size (xlarge). 

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

### Storage types
Structured data would live in a DB. 
Different types of storage - file, block and object. Block divvys up 1 gb and stores it in blocks. In object storage it uses the entire 1 gb as one block. Object is write once, read often. 
1. File storage: files stored in a heirarchy. 
2. Blocks storage: Data stored in fixed-size blocks.
3. Object storage: stored as objects in buckets. 

*File storage* is ideal when you require centralized access to files that must be easily shared and managed by multiple host computers.

Use cases include:
1. Web serving
2. Analytics
3. Media and entertainment
4. Home directories

*Block storage* splits files into fixed-size chunks of data called blocks that have their own addresses.

Use cases include:
1. Transactional workloads
2. Containers
3. VMs

*Object storage* are stored in a bucket using a flat structure, meaning there are no folders, directories, or complex hierarchies.

Use cases include:
1. Data archiving
2. Backup and recovery
3. Rich media

### File storage with EFS and FSx
Amazon Elastic File System (Amazon EFS) is a set-and-forget file system that automatically grows and shrinks as you add and remove files.

Amazon FSx is a fully managed service that offers reliability, security, scalability, and a broad set of capabilities that make it convenient and cost effective to launch, run, and scale high-performance file systems in the cloud. Can be used with Lustre, NetApp, ONTAP, OpenZFS, and Windows File Server. 

### Block Storage with EC2 Instance Store and EBS
When creating an EC2 you get an instance store from EBS. EC2 instance store is tied to the lifecycle of the EC2. If you shut the EC2 instance down the data is gone. It's ephemeral. 

Instance Store
Instance store is ideal if you host applications that replicate data to other EC2 instances, such as Hadoop clusters. It’s also ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content.

EBS
Amazon Elastic Block Store (Amazon EBS) is block-level storage that you can attach to an Amazon EC2 instance. It is:
1. Detachable: You can detach an EBS volume from one EC2 and attach it to another one in the same AZ. 
2. Distinct: It's separated out so if the EC2 goes down your data persists. 
3. Size-limited: There is a max amount of data that can be stored.
4. 1-to-1 connection: no longer true techinically. You can attach EBS volumes to multiple instances now. It's called multi-attach. 

Scaling EBS
Increase the volume size or attach multiple volumes. 

Use cases include:
1. OS: boot and root volumes. AMIs are ususally EBS volumes
2. Databases: storage layer for DBs
3. Enterprise apps: 
4. Big data analytics engines

EBS can be either SSDs or HDDs. 

Benefits include High Availbility, Data persistence, data encryption, flexibility and backups.  

EBS snapshots are incremental backups that only save the blocks on the volume that have changed after your most recent snapshot.

### Object storage with S3
Storing photos is a good use case for S3. The block drive would eventually hit the size limit on EBS. S3 is not mounted onto the EC2 instance. 

S3 Bucket
Store objects into a bucket. Folders can exist within a bucket. Buckets are region specific. 

Most objects will be denied access when first trying to retrieve them. You need to configure who can see the object url. You'll need an ACL and then "make public with ACL". Use IAM or S3 Bucket policies to decide who can see what content in an S3. 

Similar to IAM it's JSON and lists out what actions are allowed or denied on a bucket. Cannot be used on folders or individual urls. 

- Bucket names must be between 3 (min) and 63 (max) characters long.
- Bucket names can consist only of lowercase letters, numbers, dots (.), and hyphens (-).
- Bucket names must begin and end with a letter or number.
- Buckets must not be formatted as an IP address.
- A bucket name cannot be used by another AWS account in the same partition until the bucket is deleted.

Object key names
It consists of a bucket name, prefix and object key. These come together in the form of a url. 

Use cases include:
1. Backup and storage
2. Media hosting
3. Software delivery
4. Data lakes
5. Static websites
6. Static content

Security
You should use IAM policies for private buckets in the following two scenarios:

- You have many buckets with different permission requirements. Instead of defining many different S3 bucket policies, you can use IAM policies.
- You want all policies to be in a centralized location. By using IAM policies, you can manage all policy information in one location.

You should use S3 bucket policies in the following scenarios:

- You need a simple way to do cross-account access to Amazon S3, without using IAM roles.
- Your IAM policies bump up against the defined size limit. S3 bucket policies have a larger size limit.

S3 allows for versioning. If you reupload the same file with the same name it will overwrite the existing file. Versioning avoids conflicting naming and allows for preservation. Enabling it provides a UUID for each object. Helps prevent accidental deletions or overwrites. 

Version states include Unversioned, Versioning-enabled, Versioning-suspended. 

Defining a lifecycle config - options include:
1. Transition actions define when objects should transition to another storage class.
2. Expiration actions define when objects expire and should be permanently deleted.
Use cases include periodic logs or data that changes in access frequency. Good for setting rules on when to delete things after a set time. 

### Demo
Creating an S3 bucket and attaching to EC2 instance
1. Create S3 bucket (keep defaults)
2. Test uploading of an object (like an image)
3. Access the bucket via the application by using permission.
    a. Edit the bucket policy by adding the correct JSON. 
4. Authorize EC2 to access the S3 bucket. 
    a. Relaunch the EC2 using "Launch more like this"
    b. Change auto-assign public IPs to enable
    c. Advanced details > In the user data add the bucket name.
5. 2/2 checks should be passed when instance is running. Check via IP address that app is still working. 

Cloudfront can be used to deliver content of S3 using a cache

TODO find example bucket policy JSON. 

## Module 5: Databases on AWS

### Intro to DBs on AWS
RDS or Relational Database Service covers relational databases
RDBMS is a relational db management system. It covers MySQL, Postgres, Oracle, Microsoft SQL Server and Amazon Aurora. 
It allows for:
1. Complex SQL
2. Reduced redundancy
3. Familiarity
4. Accuracy

Use cases include:
1. Apps that have a fixed schema
2. Apps need persistent storage

Can be run as managed or un-managed. In the un-managed scenario you have control over almost everything, including patching and dependency installs. AWS would take care of the OS, server, rack, power, etc. 
With a managed DB more of the responsibility gets moved onto AWS. AWS takes care of patching and installing, backups, scaling, and availability. You would focus on app optimization including database tuning, query optimization and security. 

### Amazon RDS
Demo:
1. Go to RDS in AWS
2. Use Easy create
3. AWS Aurora is useful for scaling and can be used with MySQL or Postgres. 
4. Create the instance

When creating an RDS it's similar to EC2 in that the instance is created in 1 subnet inside 1 AZ. (should have 2 for availbility concerns) Multi-AZ deployment allows for this. Failover is handled automatically. 

RDS uses Elastic Block Store volumes for database and log storage. When using Aurora the data is stored in cluster volumes. 

RDS can use 3 different types of storage:
1. General Purpose SSD
2. IOPS SSD
3. Magnetic

When creating a DB you need a VPC for it to live in, as well as subnets. You can control access further with ACLs. 

Backing up data: There are 2 options
1. Automated Backups: On by default. Backups are made during the backup window. (usually when not a peak hours). Backups live between 0-35 days. Allows for point-in-time recovery so the db can be restored from a specific point in time. 
2. Manual snapshots: Needed to keep backups for more than 35 days. Takes snapshots but can store them however long you like. 

It's recommended to use both of the above options. 

Redundancy with RDS Multi-AZ:
Gives you a primary copy in one AZ and a standby copy in another AZ. If something happens to the primary DB an automatic failover is initiatied. Once the failover happens you have 2 ways to handle setting up a new standby copy. 
1. Demote the previous primary to the standby if it's still running
2. Stand up a new standby db instance. 

You can use IAM & security groups to manage access. RDS encryption secures the db instance and snapshots at rest. Option for SSL or TLS connections. 

### Purpose Built DBs
Pick a DB that fits your data. Do not change your data to fit the database. There are a lot of choices including:

1. DynamoDB is a fully managed NoSQL database that provides fast, consistent performance at any scale. DynamoDB has become the database of choice for two categories of applications: high-scale applications and serverless applications.
2. ElastiCache is a fully managed, in-memory caching solution. Supports Redis and Memcache
3. MemoryDB is a Redis-compatible, durable, in-memory database service that delivers ultra-fast performance.
4. Amazon DocumentDB is a fully managed document database from AWS. Useful for content management systems, profile management, and web and mobile applications. Supports MongoDB. 
5. Amazon Keyspaces is a scalable, highly available, and managed Apache Cassandra compatible database service.
6. Neptune is a fully managed graph database offered by AWS. Useful for recommendation engines, fraud detection, and knowledge graphs.
7. Timestream is a fast, scalable, and serverless time series database service for Internet of Things (IoT) and operational applications. It is used for measuring events that change over time, such as stock prices over time or temperature measurements over time.
8. Quantum Ledger Database is a purpose-built ledger database that provides a complete and cryptographically verifiable history of all changes made to your application data.

### DynamoDB
Serverless DB
No tables like a RDB. It uses standalone tables. It uses items and those have attributes. Doesn't require a schema or relationships. Allows for flexible schema. Good for datasets that have variation. Scales up to 10 trillion requests per day. NOSql based. 

Demo:
1. Create a new table + select the unique id

With DynamoDB, you can do the following:

- Create database tables that can store and retrieve any amount of data and serve any level of request traffic. 
- Scale up or scale down your tables' throughput capacity without downtime or performance degradation. 
- Monitor resource usage and performance metrics using the AWS Management Console.

Contains tables, which contain items, which contain attributes. 

Use cases include:
1. Developing software apps
2. Creating media metadata stores
3. Scaling gaming platforms
4. Deliver retail experiences

You might want to consider using DynamoDB in the following circumstances:
- You are experiencing scalability problems with other traditional database systems.
- You are actively engaged in developing an application or service.
- You are working with an OLTP workload.
- You care deploying a mission-critical application that must be highly available at all times without manual intervention.
- You require a high level of data durability, regardless of your backup-and-restore strategy.

Security:
- Provides redundantly stored data on multiple devices across multiply regions.
- Full encrypted at rest using keys stored in AWS Key Management Service. 
- IAM for controlling resources. 
- Global network security procedures. 

You can track usage like:
- Key usage telling you who made a request, actions performed, etc. 
- Need valid IAM creds to make API requests
- IAM policies for fine-grain control
- Monitor operations using CloudTrail

### Demo
1. Go to EC2 and launch a running instance using "launch more like this"
2. Make sure the instance is using auto-assign public IP
3. Launch instance and wait for it to be Running + checks passed. Check app is still running correctly
4. Go to DynamoDB and create a table using "id" as the primary key
5. Use the app
6. Check that S3 was updated with picture and check DB that entry was created. 

## Module 6: Monitoring, Load balancing & Scaling

### Monitoring
Each datapoint is a metric
Metrics over time are statistics
Usually you'll see CPU utilization, NetworkIn and NetworkOut. 
Determine a baseline and then alert when things leave the baseline. 

CloudWatch montiors everything in one place. 

Monitoring helps answer questions like:
- How many people are visiting my site day to day?
- How can I track the number of visitors over time?
- How will I know if the website is having performance or availability issues?
- What happens if my Amazon Elastic Compute Cloud (Amazon EC2) instance runs out of capacity?
- Will I be alerted if my website goes down?

S3 metrics look at size of objects, number of objects in a bucket, number of requests made to a bucket
RDS metrics look at DB connections, CPU utilization and disk space consumption
EC2 metrics look at CPU util, network util, disk performance and status checks. 

Monitoring allows you to:
1. Proactively respond to issues
2. Improve performance and reliability
3. Recognize security threats
4. Make data-driven decisions
5. Create cost-effective solutions

### AWS CloudWatch
Demo:
1. Start with creating a dashboard
2. Select a widget - line graph for cpu util
3. Select service and then the CPU util 
4. You can create custom metrics if you'd like
5. Next look at creating alarms
6. Create alarm and the period the alarm fires off of should be changed over time. Not too long, not too short. 
7. 3 states for an alarm - In Alarm, OK and Insufficient data
8. Can create alarms that send emails to alert certain users. 

CloudWatch can do the following:
- Detect anomalous behavior in your environments.
- Set alarms to alert you when something is not right.
- Visualize logs and metrics with the AWS Management Console.
- Take automated actions like scaling.
- Troubleshoot issues.
- Discover insights to keep your applications healthy.

More detailed monitoring costs $$$. 
Custom metrics can be setup to cover things like:
- Webpage load times
- Request error rates
- Number of processes or threads on your instance
- Amount of work performed by your application

Metric data can be consumed outside of AWS using the GetMetricData API.

Some services provide automatic logging (like Lambda). Others like EC2 will need to be configured using CloudWatch Logs. Logs contain an Event (timestamped), Log stream (grouped logs for a instance) and Group (all the log stream of the same retention and permission settings)

### Solution Optimization
Add redundancy in multiple AZs
Scale horizontially and not vertically. 
Use autoscaling to scale based on conditions. 
Use Elastic Load Balancing to handle things when the instances begin to grow. 

Challenges when managing multiple AZ
1. Replication process - manage config files, patching, etc. across all the instances. 
2. Customer redirection - use a load balancer to avoid propagation time issues. 
3. High availability: active-passive vs. active-active. 
    a. Active-passive - only one instance of 2 will be available. This doesn't scale well. 
    b. Active-active - Both instances are running and the load is spread across the 2 instances. Good for stateless applications, not good for stateful apps. (ex. customer session isn't available on both servers)

### Traffic Routing with Elastic Load Balancing
ELBs are regional services
ELBs automatically scale based on traffic
There's an App load balancer, Network load balancer, and gateway Load balancer. 
ALBs are configured using a listener (port 80/443 and protocol http https), target group (EC2, Lambda, etc), and rule (how requests are routed to a target). 

Demo:
1. ELB lives in EC2
2. Create load balancer
3. Select type
4. Select the VPC + AZs + security groups + subnets
5. Security group selection
6. Next select the Target type
7. Select instances and create the target group
8. Back on the other page select the newly created target group
9. Grab the DNS ip address 

Most loadbalancers use a round robin algo. 
Client sends request from browser > request goes to load balancer > request is forwarded to EC2 instance > repsonse goes through the load balancer > back to client. 

Different features includes:
- Hybrid mode: can manage on-prem services
- High availability: runs across multiple AZs
- Scalability: scales to meet demand of incoming traffic. 

ELBs do health checks before sending traffic. 2 different checks are:
1. Establishing a connection to a backend EC2 instance using TCP and marking the instance as available if the connection is successful.
2. Making an HTTP or HTTPS request to a webpage that you specify and validating that an HTTP response code is returned.

One strategy for defining your own health check is to create a "/monitor" page. It will make a call to the DB to make sure it can connect. This way the health check verifies the entire app is working, rather than just getting a 200 OK from the server. 

Different Load Balancers
Application Load Balancer contains feature for: (Layer 7 OSI)
- Routing traffic based on request data: granular routing to target groups
- Sends responses directly to client: useful for redirects
- TLS offloading: SSL cert must be provided by IAM or ACM
- Authenticate users: auth before user passes through load balancer
- Secure traffic: only support certain IP ranges
- Support for sticky sessions: uses cookie to remember which server to send traffic to. 

Network Load Balancer contains features for:
- TCP and UDP traffic (Layer 4 OSI)
- Sticky sessions: same client > same target
- Low latency
- Source IP address: Preserves client-side source IP address
- Static IP support: static IP per AZ (subnet)
- Elastic IP support: assign custom, fixed IP per AZ (subnet)
- DNS Failover: Route53 to direct traffic to load balancer nodes

Gateway load balancer contains features for:
- Supporting third-party apps like firewalls, intrusion detection and prevention, deep packet inspection. 
- High availability: only route through health virtual appliances
- Monitoring: supports CloudWatch
- Streamlined deployment: supports AWS marketplace
- Private connectivity: connects via VPCs, internet gateways and other resources

### EC2 Auto Scaling
Allows for provision based on demand using CloudWatch
Allows for horizontal scalability

Demo:
1. Create launch template
2. Select auto-scaling option
3. Select instance type to match the existing instance
4. Select security group
5. Advanced details to select IAM role
6. Paste user data from the other template
7. Next define autoscaling group (side panel)
8. Use the template we just defined
9. Select the VPC for the network and the 2 subnets
10. Select the existing load balancer
11. Select target group and enable the health checks. 
12. Select minimum and max instances and desired (defaults to min)
13. Scaling policies - where you'd use CloudWatch to determine scaling
    a. Similar to setting up an alarm
14. Can add notifications when a scaling event happens
15. Stress app to see it works (load testing tool in real world)
16. Look at CloudWatch to see if instances fire up under stress
17. Look at target groups in the load balancing to see all the instances launched. 

Vertical scaling increases resources for each instance. Steps include stopping the instance, changing the instance size or type, shifting traffic to instance and making it active, and make sure both instances match the change. 

Horizontal scaling is simply adding more instances using the auto-scaling service. Features include:
- Auto scaling
- Schedule based scaling based on user-defined schedules
- Fleet management: auto replace unhealthy instances
- Predictive scaling: ML driven schedule to optimize number of instances. 
- Purchase options
- Integrates with ELB

Launch templates define what is launched when scaling. You can create a launch template in one of three ways as follows:
- Use an existing EC2 instance. All the settings are already defined.
- Create one from an already existing template or a previous version of a launch template.
- Create a template from scratch. These parameters will need to be defined: AMI ID, instance type, key pair, security group, storage, and resource tags.

Don't use launch configs. 

Autoscaling groups define where in EC2 the resources get deployed. Uses the VPC and subnets for this. Need at least 2 AZs. 

Set a max to avoid getting charged a lot of $$$. 

Scaling policies is where you can use CloudWatch to define the actions of the scaling. 3 policies are:
- Simple Scaling Policy: Has a cooldown period to make sure you don't infinitely scale. 
- Step Scaling Policy: Defines rules for scaling that would trigger even while scaling activity or health checks are in progress. 
- Target Tracking Scaling Policy: Easiest one to use when using default target tracking. Creates things automatically in CloudWatch for you. 

### Demo App Redesign


