# Migrating-a-Monolithic-Application-into-Microservices-
## Project Overview
This project involves the architectural transformation of a leading global e-commerce platform that offers a diverse range of products to customers worldwide. Originally developed as a monolithic application, the platform integrates key functionalities such as user authentication, product catalog management, order processing, and payment services within a single, tightly coupled codebase.
To enhance scalability, agility, and deployment efficiency, the goal of this project is to refactor the monolithic architecture into a microservices-based architecture. This shift will enable the independent development, deployment, and scaling of each core service, ultimately improving system resilience, team productivity, and time-to-market. In this project we would leverage AWS Copilot, Amazon ECS, Docker, and AWS Fargate to achieve this.

## Objectives
In this Project, you will deploy a monolithic node.js application to a Docker container, then decouple the application into microservices without any downtime.
The node.js application hosts a simple message board with threads and messages between users.


## Key Concepts
### What Is a Container?
Containers allow you to easily package an application's code, configurations, and dependencies into easy-to-use building blocks that deliver environmental consistency, operational efficiency, developer productivity, and version control. Containers can help ensure that applications deploy quickly, reliably, and consistently, regardless of the deployment environment
Launching a container with a new release of code can be done without significant deployment overhead. Operational speed is improved because code built in a container on a developer’s local machine can be easily moved to a test server by simply moving the container. At build time, this container can be linked to other containers required to run the application stack.

### AWS Copilot
AWS Copilot, a user-friendly command-line interface, streamlines the entire lifecycle of containerized applications, making it easier for developers to focus on building features rather than managing infrastructure.

### Amazon ECS (Elastic Container Service)
Amazon ECS is a highly scalable, high-performance container management service that supports Docker containers and allows you to run applications on a managed cluster of Amazon EC2 instances. With simple API calls, you can launch and stop Docker-enabled applications, query the complete state of your cluster, and access many familiar features like security groups, Elastic Load Balancing, EBS volumes, and IAM roles.
You can use Amazon ECS to schedule the placement of containers across your cluster based on your resource needs and availability requirements. You can also integrate your scheduler or third-party schedulers to meet business or application-specific requirements.

### Docker
Docker, a widely adopted containerization platform, ensures consistency in deployment across diverse environments, promoting seamless collaboration and integration.

### AWs Fargate
As the foundation of this microservices architecture, AWS Fargate enters the scene to abstract away the complexities of infrastructure management. By providing on-demand, serverless compute for containers, Fargate empowers developers to focus solely on their applications without the burden of server provisioning or maintenance.

The integration of AWS Copilot, Amazon ECS, Docker, and AWS Fargate forms a potent alliance, offering organizations the tools needed to successfully break down monolithic barriers, embark on a microservices journey, and unlock the full potential of cloud-native development on the AWS platform. This cohesive ecosystem facilitates a smoother transition and positions businesses to thrive in the ever-evolving landscape of digital innovation.

## Application Architecture
### Monolithic Architecture
The entire Node.js application is run in a container as a single service, and each container has the same features as all other containers. If one application feature experiences a spike in demand, the entire architecture must be scaled.
### Microservices Architecture
Each feature of the Node.js application runs as a separate service within its container. The services can scale and be updated independently of the others.
You will build the container image for your monolithic Node.js application and push it to the Amazon Elastic Container Registry

### Importance of Microservices
Traditional monolithic architectures are hard to scale. As an application's code base grows, it becomes complex to update and maintain. Introducing new features, languages, frameworks, and technologies becomes very hard, limiting innovation and new ideas.
Within a microservices architecture, each application component runs as its service and communicates with other services via a well-defined API. Microservices are built around business capabilities, and each service performs a single function. Microservices can be written using different frameworks and programming languages, and you can deploy them independently, as a single service, or as a group of services.
#### Isolation of crashes:
Even the best engineering organizations can and do have fatal crashes in production. In addition to following all the standard best practices for handling crashes gracefully, one approach that can limit the impact of such crashes is building microservices. Good microservice architecture means that if one micropiece of your service is crashing, then only that part of your service will go down. The rest of your service can continue to work properly.

#### Isolation for security:
In a monolithic application, if one feature of the application has a security breach, for example, a vulnerability that allows remote code execution, then you must assume that an attacker could have gained access to every other feature of the system as well. This can be dangerous if, for example, your avatar upload feature has a security issue that ends up compromising your database with user passwords. Separating features into microservices using Amazon ECS allows you to secure access to AWS resources by giving each service its own AWS Identity and Access Management (IAM) role. When microservice best practices are followed, the result is that if an attacker compromises one service, they only gain access to the resources of that service and cannot horizontally access other resources from other services without breaking into those services as well.

#### Independent scaling: 
When features are broken out into microservices, then the amount of infrastructure and number of instances used by each microservice class can be scaled up and down independently. This makes it easier to measure the cost of a particular feature and identify features that may need to be optimized first. If one particular feature is having issues with its resource needs, other features will not be impacted, and reliable performance can be maintained.
Development velocity

Microservices lower the risks in development, which can enable a team to build faster. In a monolith, adding a new feature can potentially impact every other feature that the monolith contains. Developers must carefully consider the impact of any code they add and ensure that they do not break anything. On the other hand, a proper microservices architecture has new code for a new feature going into a new service. Developers can be confident that any code they write will not be able to impact the existing code at all unless they explicitly write a connection between two microservices.
