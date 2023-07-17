# AWS Compute Servies Overview

## Section 1: AWS Compute Options

Main choices are instances, containers and serverless. 
Compute mainly uses compute power including:
- CPI: measured in millicores. App devs specify how many CPIs are allocated. 
- Memory: Measured in bytes. On-prem can run into limitation, but cloud computing allows for additional allocation if memory runs out. 

EC2 uses instances (VMs)
Containers package everything up and allow for quick, reliable, consistent deployment. 
Container servies are EKS for K8s allows for automated deployment, scaling and management of containerized apps. 
ECS is Elastic Containe Service and allows for container management across a cluster of EC2 instances. 
Serverless is convenience focused without thinking about servers. 
Lambda functions runs code without provisioning or managing servers. 

## Section 2: Selecting the right option
EC Features:
With Amazon EC2, you can quickly build and start a new server: You don't need to rack the server, run cable, and update hardware drivers as you would do with a traditional server.

- You can scale capacity as needed, both up and down. This means that if you need more memory, processing, or storage, you can add it.

- Instances offer at least 99.99% (four nines) of availability. For more information on AWS and availability, see Amazon Compute Service Level Agreement.

- Amazon EC2 offers instances that are optimized for specific types of workloads, including memory optimized, compute optimized, storage optimized, accelerated computing, and general purpose.

- Various instance types are available with different pricing options, so you can choose the best option to fit your business requirements. These options include On-Demand Instances, Reserved Instances, and Spot Instances.

- Amazon EC2 gives you complete control over the instance, down to the root level. You can manage the instance as you would manage a physical server.

- You can use instances for long-running applications, especially those with state information and long-running computation cycles.

Container features:
- The application is packaged so that you control the application and all associated resources, such as policies, security, and deployment.

- Containers are portable and can be moved to different OS or hardware platforms, and through different environments such as development, testing/staging, pre-production, and production.

- There are no time-out limits when running. This is useful for applications that run longer than 15 minutes or that need to initiate instantly when called.

- Containers run without the startup latency of Lambda or Amazon EC2.

- Containers have no size limit. They can be as large or as small as you need them to be.

- Containers are useful when taking a large traditional application and breaking it down into small parts, or microservices, to make the application more scaleable and resilient. 

Serverless features:
- Fast development
- Pay for value
- Short lived apps
- Event-driven apps
- Auto scaling
- Redundancy and resilience

Choosing correctly
Choose EC2 for:
- When you have compute-intensive or memory-intensive applications, consider the following:
    - You can select a specific instance type based on the requirements of the application or software that you plan to run on your instance. 
        - Amazon EC2 provides each instance with a consistent and predictable amount of CPU capacity, regardless of its underlying hardware.
    - Amazon EC2 gives you the choice of OSs and software packages. You can select a configuration of memory, CPU, instance storage, and a boot partition size that is optimal for your choice of OS and application.
    - You have access to the underlying files of the instance to customize or update as needed.
- Amazon EC2 works with other AWS services to provide a complete solution for computing, query processing, and storage across a wide range of applications.
    - From a performance perspective, you can use instances for long-running, stateful applications.
    - You can determine the type and size or your storage, whether to use block or file storage.
    - Amazon EC2 can support workloads where complex networking, security, and storage is required, in addition to applications and workloads with heavy computational requirements.
- EC2 instances are optimized for use cases such as compute optimized, storage optimized, general purpose, or accelerated computing.
- Amazon EC2 is versatile and reliable. You can run a simple website or you can run a vast, complex, custom-built application, and when necessary, replacement instances can be rapidly and predictably deployed. 
- Pricing is pay for what you use. There are also different types of pricing models so you can select the type of instance and the pricing model that fits your budget.

Choose containers for:
- For compute-intensive workloads
    - Applications that are compute intensive run better in a container environment. If you have a small application that runs under in 15 minutes but is compute intensive, consider using a container. Lambda is not the best fit for a heavily compute-intensive piece of code.
- For large monolithic applications 
    - These are appropriate candidates to move to containers. Large monoliths that have many parts are very suitable applications to consider moving to containers. 
    - *You can break apart applications and run them as independent components, called microservices, using containers to isolate processes.*
        - By using microservices, you can break large applications into smaller, self-contained pieces. 
        - Segmenting a larger application means that updates can be applied on only specific parts. Because each part of the larger application resides in its own container, an update that might have affected files used by a different piece of the application is isolated to its own container.
        - With containers, you can do frequent updates by pushing out new containers, without the concern that one set of updates might break another part of the application. If you detect any issues, you have the flexibility to undo the change quickly.
- When you need to scale quickly 
    - Containers can be built and taken down quickly, which means fast application deployment and scaling.
- When you need to move your large application to the cloud without altering the code 
    - With containers, you can package entire applications and move them to the cloud without the need to make any code changes. 
    - Your application can be as large as you need it to be and can run as long as you require it to run.

Don't use containers:
- When applications need persistent data storage
- When applications have complex networking, routing, or security requirements

Choose Serverless for:
- Are you building, testing, and deploying applications frequently and want to focus only on your code and not on infrastructure?
- Are your applications less compute intensive?
- Are the applications that you are running or building small, simple, or modular?
    - Simple applications, such as chatbots or to-do lists that people can use to modify a list of things that they need to do, are good choices to move to serverless.
- Will you be using multiple AWS services where one service might need to call another service? 
    - For example, if someone uploads a file to Amazon Simple Storage Service (Amazon S3), will you then need to invoke other workflows to log the update or convert the file to HTML?
- Do your applications finish quickly?
    - Serverless is most suitable for applications that don't run longer than 15 minutes.

-------------------------

Other Compute options
- AWS Step functions: AWS Step Functions is a fully managed service that you can use to coordinate the components of distributed applications and microservices using visual workflows.
- AWS Batch: AWS Batch is a set of batch management capabilities that you can use to efficiently run hundreds of thousands of batch computing jobs on AWS.
- AWS Elastic Beanstalk: With Elastic Beanstalk, developers upload their application. Then, Elastic Beanstalk automatically handles the deployment details of capacity provisioning, load balancing, auto-scaling, and application health monitoring.
- AWS Lightsail: Lightsail is a VPS provider and is a useful way to get started with AWS for users who need a solution to build and host their applications on AWS Cloud. This includes VMs, containers, databases, content delivery network (CDN), load balancers, Domain Name System (DNS) management, and so on. 

How to choose from all these options?
1. What allows you to build fast and start generating revenue?
2. Early on don't worry about scale. 
3. For a startup starting from scratch. Serverless would be best. 
4. Currently already (docker) containerized app. ECS or EKS. Fargate allows for scaling. 
5. Existing monolithic app. Elastic beanstalk allows for an experienced team to leverage full access.
A lot of teams use a combination of all of these. 