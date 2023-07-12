# AWS Partner: Technical Accreditation

Compute
EC2 servers with an option for autoscaling
AMI(Amazon Machine Images)
EC2 instances are the building blocks
Apply AMIs to customize
Congure auto-scaling and load balancing
Manages costs

Elastic Load Balancing to distribute traffic
Elastic Container Service allows for docker and runs in a cluster (like K8s)
Elastic K8s Service is specifically for K8s
AWS Lambda is event driven


Storage
S3 - simple storage service

Elastic Block Store - block level storage (volume that can be connected to an EC2) EBS retain data after EC2 is terminated. Allows for snapshots to be stored in S3

S3 Glacier - data archiving and backup
Storage Gateway - integration for moving large data in and out of AWS
Elastic File System - file storage for EC2 instances
FSx is file storage for widely used file systems

Databases
RDS relational DB (Postgres included)
DynamoDB nosql db is fast for performance
Elastic Cache is fast for retrieving data from in-memory cache
Better to do this than running the DB on your own EC2. Automatic backups, provisioning, etc. 
When managing on an EC2 you get better control, access and features that aren’t supported by AWS

Replace daily management task with value-added processes

Networking
VPC - virtual network in the cloud
Much like a VPN a company would run on premise
Subnet is a range of IP addresses allowed

Security groups - control access to instances
Network Access Control Lists - control access to subnets
Flow logs can capture network flow info for trouble shooting

Route 53 - route end users to internet apps
VPC is the starting point and all the other things here provide additional permissions to the instances

Security

This is a shared responsibility as some of the Customer security is up to us

IAM identity and access management controls who can see AWS services
Infrastructure Protection
Identity and access management
Detection
Data Protection
Compliance
Incident Response

————————

Solution Design

Get customer’s current architecture
List of challenges
List of ideas for solutions and improvements
Are there any case studies showing a successful solution and how it looks?

SEVEN Rs of a migration
Rehost
On premise is usually done as a Rehost
Recreate everything on AWS. Application Migration service is an option

Replatform
Retain current infra but chance certain things. Example: Only move the DB to RDS. Also includes optimizations as well. 

Relocate
Usually only done with VMware infrastructure

Refactor
Reimage how the app is architected using cloud features. Example: moving DB from RDS to using Lambda or other features. 

Retire
After an audit some apps can simply be shut off

Retain
Keep the existing architecture (example: customer just updated their app recently). 

Repurchase
Moving workflows to a SaaS product. 

BEST PRACTICES
Design for failure - avoid single points of failure, multiple instances, multiple availability zones, separate single server into tiered applications. RDS use multi-AZ
Build security at every layer - encrypt data at rest and in transit, enforce principle of least privilege in IAM, implement security and network access control lists, consider advanced security features
Leverage different storage options - move static assets to S3, Cloudfront for global serving, store session state, ElastiCache to lower load on DB
Implement elasticity -  implement autoscaling, architect resiliency to reboot and relaunch, leverage managed services like S3 and DynamoDB
Think parallel - scale horizontally, not vertically, decouple compute from session/state, use load balancing, right-size your infra
Loose coupling sets you free - instead of a single, ordered workflow use multiple queues, use Simple Queue Service and Simple notification service and leverage existing services
Don’t fear constraints - rethink traditional constraints

See the Well-Architected Framework from AWS (there’s also a Well-Architected Tool). AWS also has case studies and quick starts
—————————

Presenting the solution
Discovery - listen and understand. Encourage conversation and use open ended questions. 5 Why’s - continue asking why. Whiteboarding. Next steps

Presentation
POC
