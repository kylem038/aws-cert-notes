# AWS Technical Essentials

## What is AWS?
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

## AWS Global infrastructure
Regions, Availability Zones, and data centers exist in a redundant, nested sort of way. There are data centers inside of Availability Zones and Availability Zones inside of Regions. And how do you choose a Region? By looking at compliance, latency, pricing, and service availability. 

There are also global edge networks. Edge locations and regional edge caches are used to cache content closer to end users, thus reducing latency. You can use services like Amazon CloudFront to cache content using the edge locations.

A well-known best practice for cloud architecture is to use Region-scoped, managed services. These services come with availability and resiliency built in. When that is not possible, make sure your workload is replicated across multiple Availability Zones. At a minimum, you should use two Availability Zones.

## Interacting with AWS
Three main ways to interact with AWS are the Management Console, CLI or SDK. 

The management console is a good starting place but doesn't offer the ability for scripting or doing things programmatically. The CLI is good for that. 

Example: You want to run a report to collect data from all the servers. You need to do this programmatically every day because the server details might change. Instead of manually logging in to the console and then copying and pasting information, you can schedule an AWS CLI script with an API call to pull this data for you.

Developers commonly use AWS SDKs to integrate their application source code with AWS services. For example, consider an application with a frontend that runs in Python. Every time the application receives a photo, it uploads the file to a storage service. This action can be achieved in the source code by using the AWS SDK for Python (Boto3).

## Security and the Shared Responsbility Model
AWS covers the security "of" the cloud. We're responsible for security "in" the cloud. 

We're responible for things like IAM, updating OS and firewalls, encryption of client-side data, server-side encyption and networking traffic protection. 

AWS handles the hardware and infra security and the software running on the hardware (compute, storage, database and networking)