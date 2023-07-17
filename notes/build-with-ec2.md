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