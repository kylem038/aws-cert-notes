# Build With Amazon EC2

## Section 1: Fundamentals
When listing the additional attributes an EC2 family can have they include:
a – AMD processors
g – AWS Graviton processors
i – Intel processors
d – Instance store volumes
n – Network optimization
b – Block storage optimization
e – Extra storage or memory
z – High frequency

## Section 2: Intro to AMIs (Amazon Machine Images)
An Amazon Machine Image (AMI) is a template that contains the software configuration—for example, an operating system (OS), applications, or tools.

An AMI will contain:
1. Storage: EBS-backed volume including 1+ EBS snapshots, or for an instance store-backed AMI it'll contain the root volume of the instance. 
2. Permissions: launch controls for which AWS account can use the AMI
3. Mapping: Block device mapping that specifies any additional EBS volumes to attach to the instance on launch. 

EC2 is made up of hardware components, hypervisor and config. A hypervisor is a piece of virtualization software that allocates resources and manages physical hardware in a virtualized environment. The hypervisor allows multiple operating systems to run on a single physical server. 

Newer offering is the AWS Nitro Hypervisor. 
--------
AWS Nitro System
The majority of EC2 instances run on hardware known as the AWS Nitro System. The AWS Nitro System is built specifically to run instances in the most optimal fashion possible. It uses dedicated hardware to offload I/O operations. 

Traditionally, a hypervisor's job is to protect the physical hardware and BIOS; virtualize the CPU, storage, and networking; and provide a rich set of management capabilities. With the Nitro System, these functions are separated and offloaded to dedicated hardware and software.
--------
Network Interface
Each instance comes with an elastic network interface. This is a logical networking component that represents a virtual network card.
--------
AWS also offers the use of bare-metal instances.
Bare-metal instances are ideal for the following applications: 

- Workloads that require access to the hardware feature set
- Applications that need to run in nonvirtualized environments for licensing or support requirements
- Customers who want to use their own hypervisor

Benefits include:
- Deep performance analysis tools
- Specialized workloads that require direct access to bare-metal infrastructure
- Legacy workloads not supported in virtual environments
- Licensing-restricted Tier 1 business-critical applications

A bare-metal instance has the lowest latency because it does not have the overhead of running a hypervisor.

## Section 3: EC2 Environment

### EC2 Instance Tenancy
A tenant is the most fundamental concept in the cloud. A tenant is an entity that occupies space, whether that space is a rented apartment in a building you own, or if that rented space is an instance occupying resources on AWS infrastructure. With Amazon EC2, tenancy defines how the EC2 instances are distributed across the physical hardware. Tenancy choices also have an effect on pricing. 

Three options:
1. Shared (default) - Multiple AWS account share the same hardware
2. Dedicated instance - Your instances runs on single-tenant hardware. 
3. Dedicated host - Instance runs on physical server with EC2 instance capacity fully dedicated to your use. Has isolated physical server configs to control. 

### Shared tenancy
Shared tenancy is the most economical choice and can support Spot Instances and burstable instance types. Shared tenancy doesn’t support instances that use the Bring Your Own License (BYOL) model. 

### Dedicated Host
An Amazon EC2 Dedicated Host is a physical server where all the instance capacity is fully dedicated to your use.

For compliance, security, or licensing reasons, some organizations must run their instances on dedicated servers.

### Dedicated Instances
Dedicated Instances are Amazon EC2 instances that run on hardware that's dedicated to a single customer. Dedicated Instances can share hardware with other instances from the same AWS account that are not Dedicated Instances

A Dedicated Host gives you additional visibility and control over how instances are placed on a physical server. And you have greater visibility into the hardware that the instance is running on. 

With a Dedicated Instance, only instances owned by you can run on the hardware, but you have no visibility into the underlying hardware nor control of instance placement. 

## Networking Fundamentals
A Region is a geographic area where AWS has deployed physical infrastructure and data centers. These data centers are arranged into logical groupings called Availability Zones.

When picking a Region consider:
1. Compliance with data governance and legal requirements
2. Proximity to customers
3. Available services in the region
4. Pricing per Region

### VPCS
A virtual private cloud (VPC) is a private virtual network dedicated to your AWS account. Remember, virtual means that the configuration is software based and does not map directly to physical infrastructure.

VPCs contain subnets where you launch resources, such as EC2 instances. 

VPC types: default and non-default
You can only have one default VPC per Region. Any additional VPCs you create are isolated and private unless you explicitly grant public access. 

Nondefault VPCs have a choice for default or dedicated tenancy. Default tenancy is any resource provisioned inside your VPC on shared hardware. As mentioned in a previous lesson, dedicated tenancy has hardware that is dedicated specifically to your account.

### AWS Networking
Contains options for:
1. Subnets: A subnet consists of a smaller portion of your IP address range. Each subnet must reside entirely within one Availability Zone and cannot span zones. By default, Amazon Virtual Private Cloud (Amazon VPC) uses the IPv4 addressing protocol; you can't turn off this behavior.
2. Subnetting (subnet masks)
    - An IP address of 10.0.1.4/16 means the following:
    10.0 is the network portion.
    .1.4 is the host portion.
    The subnet mask is /16 and looks like 255.255.0.0.
    Written out it is 11111111.1111111.0.0.
3. CIDR blocks: A CIDR block is a range of IP addresses.
4. Five reserved IPs
    - The network address: x.x.x.0
    - The first IP address after the network address: x.x.x.1 (for VPC router)
    - The second IP address after the network address (used for DNS)
    - Third IP (reserved for future use)
    - Last address cannot be used (broadcast)
5. Network access control lists
6. Security groups

Say you use /28 range (containing 16 IP addresses), only 11 can be used as the other 5 are reserved. 

Network ACLs (access control lists) filters traffics and it enters and leaves the subnet. The default network ACL allows all inbound and outbound traffic.

Security groups: Unlike network ACLs that are attached to the subnet, security groups are attached to the elastic network interface of the AWS resources within the subnet. That means the security groups would be at the boundary of the EC2 instance instead of at the subnet boundary like a network ACL. 

They are stateful, and also allow rules to be added for other security groups, or add a rule for the security group itself. 

## Section 4: Launching an EC2 instance
The most common ways to launch an instance are through the AWS Management Console, AWS Command Line Interface (AWS CLI), an AWS SDK, and as a custom Amazon Machine Image (AMI).

When your instance boots for the first time, the public key that you specified at launch is placed on your Linux instance in an entry within ~/.ssh/authorized_keys.

Because Amazon EC2 doesn't keep a copy of your private key, there is no way to recover a private key if you lose it. 

A launch template contains all the configuration information necessary to launch an instance.

## Section 5: Managing Access To Instances
Linux instances would need:
1. ID of the instance
2. Public DNS name of the instance
3. User name for the instance

Windows instanaces would need:
1. RDP (Remote Desktop Protocol) on your local machine
2. Locate the private key (get the path to the key)
3. Enable inbound RDP traffic from your IP address on the instance. 

SSH authentication to an EC2 instance is done through the use of the key pair you associated with the instance when you launched it.
SSH tools can help provide a GUI for this like OpenSSH, PuTTY, and xShell.

Controlling access to the instance uses security groups. By default, all traffic is allowed out of the instance and you must create inbound rules if you want any traffic allowed inbound. If you are using SSH to connect to your instance, you must ensure that port 22 is configured as an inbound security group rule.

EC2 Instance Connect provides a way to connect to the EC2 for Linux based instances using SSH. You use AWS Identity and Access Management (IAM) policies and principals to control SSH access to your instances. EC2 Instance Connect removes the need to share and manage SSH key pairs.

With AWS Systems Manager Session Manager, you can manage your Amazon EC2 instances through a browser-based shell or through the AWS CLI.

Session Manager removes the need to open inbound ports, manage SSH keys, or use bastion hosts.
Session Manager provides:
- Centralized access control to managed nodes using IAM policies
- No open inbound ports and no need to manage bastion hosts or SSH keys
- One- click access to managed nodes from the console and AWS CLI.
- Port forwarding
- Cross-platform suuport for Windows, Linux and Mac
- Logging and auditing session activity. 

## Section 6: Instance configuration
In EC2, two variants of the boot mode software are supported.
1. Unified Extensible Firmware Interface (UEFI)—Graviton instance types run UEFI by default.
2. Legacy BIOS—Intel and AMD instance types run on Legacy BIOS by default. 

You can purchase and use other AMIs from the marketplace, but be sure to vet them as Amazon doesn't do this. 

Launch permissions can be:
- public: all AWS accounts can launch it
- explicit: certain AWS accounts can launch it
- implicit: Only the owner of the AMI can launch it. 

EC2 Image Builder
EC2 Image Builder simplifies the building, testing, and deployment of virtual machine and container images for use on AWS or on premises. It can significantly reduce the effort of keeping images up-to-date and secure by providing a simple graphical interface, built-in automation, and AWS-provided security settings.

Considerations when sharing AMIs internally:
- No sharing limits
- Copying and usage: Usage only allows for launching the instance. Cannot delete, share or modify it. Copying an AMI must be granted access permission. 
- Tags
- Encryption and keys: The encrypted snapshots must be encrypted with a customer managed key. You can’t share AMIs that are backed by snapshots that are encrypted with the default AWS managed key. 
- Regional resource
- Billing

------------
Storage
- Instance store-backed instances: After an instance store-backed instance fails or terminates, it cannot be restored.
- EBS: An Amazon EBS-backed instance can be stopped and later restarted without affecting data stored in the attached volumes.

Block device mapping are best for temp data as the data is lost when the service is stopped. 

Instance store is similar to Block store but the data persists over the lifetime of the instance, allowing for rebotting. If the instance stops, hibernates, terminates or the disk fails the data will be lost. 

EBS Volumes is similar to attaching a drive to your laptop. It includes:
- Primary storage
- Multiple volume types
- Multiple volumes per instance
- Data availability
- Data persistence
- Data encryption
- Flexibility
- Snapshots

File storing options include S3, EFS and FSx
------------

Network Options
A primary elastic network interface is created by default when the instance is created. You cannot detach or move a primary elastic network interface from the instance on which it was created. 

A secondary elastic network interface is an additional interface that you create and attach to the instance. The maximum number of elastic network interfaces that you can use varies by instance type.

Elastic network interfaces provide failover functionality, management (one for admin traffic and one for other traffic)

When you launch an instance in a default VPC, a public IP address is assigned automatically. When you launch an instance into a nondefault VPC, it checks an attribute in the subnet that determines whether instances launched into that subnet can receive a public IP address from the public IPv4 address pool.

Elastic IP address: Unlike an auto-assigned public IP address, an Elastic IP address is preserved after you stop and start your instance in a VPC. 
- You can't retain or reserve the current public IP address assigned
- There is a default limit of five Elastic IP addresses per Region per AWS account.
- You are charged a small fee for any Elastic IP address that is not associated to a running instance. 

Bring your own IP addresses (BYOIP)

## Section 7: Designing For Resilience
EC2 settings that impact resilience
1. Termination setting: Setting persists EBS when an instance terminates. Has no effect on instance store volumes. 
    - Root volume: default action for a root volume is to delete the root volume when an instance is terminated
    - non-root volume: default action is to preserve non-root volumes when the instance is terminated. 
2. Shutdown behavior: Can set bahavior to STOP instead of TERMINATE
3. Modify termination policy: Auto Scaling has rules for defining what instances get terminated when scaling. 
4. Enable termination protection: Decide who can terminate an instance when using EC2 console, CLI or API. 
5. Simplified automatic recovery: Allows for a failed instance to be recovered. 
------------

Service Quotas
Limits the amount of resources that can be provisioned using Limit Monitor and Service Quotas. Can limit:
- Number of vCPUs you can deploy
- Bandwidth limited based on instance type and size
- Elastic IP limits
- Elastic network interface and normal IP limits
- API call limits
-------------

Recovering from failures
- Status checks for instances: runs every minute to see whether an instance is healthy
- Recover your instance: If an instance is unhealthy is can be automatically replaced with a healthy one. 
- Simplified automatic recovery: is initiated in response to system status check failures.

For an EC2 instance to be recovered fully, you need to have the EBS snapshots and a copy of the AMI used to build the instance, updated and with the applicable software versions and patches.

AWS Backup can help you backup everything you need to redeploy a downed instance

## Section 8: Selecting Purchase Options
Pricing is covered via:
1. Understanding pricing fundamentals: Some services incur costs while they're running, some only when they're used or invoked. 
2. Maximize the power of flexibility: Things can be changed to reduce costs where possible. 
3. Balancing cost and availability: Pick less permformant AMIs, or maybe change your auto scaling to allow for fewer instances. 
4. Compliance costs: Things like data retention policies will cost more. 

Things to help bring costs down:
- On-Demand Instances: Pay for compute capacity by the hour or second. (no upfront costs). Good for short term projects, longer projects benefit from yearly contracts
- Saving Plans: Savings Plans are a flexible pricing model offering lower prices in exchange for a specific usage commitment for a 1-3 year period.
- Reserved Instances (RIs) are a billing discount applied to the use of On-Demand Instances in your account. Best for steady-state usage. 

A one-time Spot Instance request remains active until Amazon EC2 launches the Spot Instance, the request expires, or you cancel the request.

A persistent Spot Instance request remains active until it expires or you cancel it, even if the request is fulfilled.

AWS provides a pricing calculator

Summary
- On-Demand Instances – You pay full price by the second when you launch.
- Savings Plans – You commit to a certain amount of usage over a 1–3-year period.
- Reserved Instances – You agree to a specific instance configuration for a period of 1–3 years.
- Spot Instances – This refers to unused instance resources that you can bid on. Your price is determined by market availability.
- Dedicated Hosts – You get a full physical server.
- Dedicated Instances - Instances are placed on a single host with no access to the underlying host.


