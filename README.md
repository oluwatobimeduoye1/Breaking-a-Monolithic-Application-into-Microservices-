# Migrating-a-Monolithic-Application-into-Microservices-
## Project Overview
This project involves the architectural transformation of a leading global e-commerce platform that offers a diverse range of products to customers worldwide. Originally developed as a monolithic application, the platform integrates key functionalities such as user authentication, product catalog management, order processing, and payment services within a single, tightly coupled codebase.
To enhance scalability, agility, and deployment efficiency, the goal of this project is to refactor the monolithic architecture into a microservices-based architecture. This shift will enable the independent development, deployment, and scaling of each core service, ultimately improving system resilience, team productivity, and time-to-market. In this project we would leverage AWS Copilot, Amazon ECS, Docker, and AWS Fargate to achieve this.

## Objectives
In this Project, a monolithic node.js application will be deployed to a Docker container, then decouple the application into microservices without any downtime.
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
### Current Monolithic Architecture
The entire Node.js application is run in a container as a single service, and each container has the same features as all other containers. If one application feature experiences a spike in demand, the entire architecture must be scaled.
This Architecture often face challenges as they scale, hindering the ability to deploy updates independently.
![Screenshot 2025-06-24 052423](https://github.com/user-attachments/assets/c95ec09d-bc4c-4d83-a802-27b85df4cf7a)

### Planned Microservices Architecture
Each feature of the Node.js application runs as a separate service within its container. The services can scale and be updated independently of the others.
        ![Screenshot 2025-06-24 053029](https://github.com/user-attachments/assets/f89e50ee-3fe2-41b0-b671-a91fcf5a8bca)


### Importance of Microservices
Within a microservices architecture, each application component runs as its service and communicates with other services via a well-defined API. Microservices are built around business capabilities, and each service performs a single function. Microservices can be written using different frameworks and programming languages, and can be deployed independently, as a single service, or as a group of services.
#### Isolation of crashes:
Even the best engineering organizations can and do have fatal crashes in production. In addition to following all the standard best practices for handling crashes gracefully, one approach that can limit the impact of such crashes is building microservices. Good microservice architecture means that if one micropiece of your service is crashing, then only that part of your service will go down. The rest of your service can continue to work properly.

#### Isolation for security:
In a monolithic application, if one feature of the application has a security breach, for example, a vulnerability that allows remote code execution, then you must assume that an attacker could have gained access to every other feature of the system as well. This can be dangerous if, for example, your avatar upload feature has a security issue that ends up compromising your database with user passwords. Separating features into microservices using Amazon ECS allows you to secure access to AWS resources by giving each service its own AWS Identity and Access Management (IAM) role. When microservice best practices are followed, the result is that if an attacker compromises one service, they only gain access to the resources of that service and cannot horizontally access other resources from other services without breaking into those services as well.

#### Independent scaling: 
When features are broken out into microservices, then the amount of infrastructure and number of instances used by each microservice class can be scaled up and down independently. This makes it easier to measure the cost of a particular feature and identify features that may need to be optimized first. If one particular feature is having issues with its resource needs, other features will not be impacted, and reliable performance can be maintained.

#### Development velocity
Microservices lower the risks in development, which can enable a team to build faster. In a monolith, adding a new feature can potentially impact every other feature that the monolith contains. Developers must carefully consider the impact of any code they add and ensure that they do not break anything. On the other hand, a proper microservices architecture has new code for a new feature going into a new service. Developers can be confident that any code they write will not be able to impact the existing code at all unless they explicitly write a connection between two microservices.
 
## Project Stages
1. Configure AWS Cli and then install AWS Copilot and Docker on Ubuntu EC2 Instance
2. Containerize and deploy the monolith imaged as a container running on managed cluster of EC2 compute intstances initiated by AWS Copilot.
3. Break the monolith: Break the Node.js application into several interconnected services and push each service's image to an Amazon Elastic Container Registry (Amazon ECR) repository.
4. Deploy microservices: Deploy your Node.js application as a set of interconnected services behind an Application Load Balancer (ALB). Then, you will use the ALB to shift traffic from the monolith to the microservices seamlessly.


## INSTALLATION OF AWS CLI, DOCKER AND AWS Copilot on UBUTU EC2 INSTANCE 
#### Install and configure AWS CLI
```
sudo apt install unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
aws configure.
```
#### Install and Start docker 
``` bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl start docker
sudo systemctl status docker
```
##### Add user to the docker group (to get priviledge to interact with docker daemon without needing to use sudo command)
$ sudo usermod -aG docker <userid>
##### Next
Then swicth to *root user*, giving you full administrative privileges
```
sudo -i
aws configure
```
#### Install AWS Copilot 
The AWS Copilot CLI is a tool for building, releasing, and operating production-ready containerized applications on Amazon ECS (Elastic Container Service).
```
curl -Lo copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux && chmod +x copilot && sudo mv copilot /usr/local/bin/copilot && copilot --help
```
Then we clone the git repository, which has all the application codes in it.i.e git clone "repostory URL"


## Containerize and Deploy the Monolith
Containerizing and deploying a monolith involves packaging your entire application and its dependencies into a container, making it portable and easy to deploy across different environments.
This process provides a scalable and manageable way to run your monolith in a containerized environment. It enhances flexibility and resource utilization and eases deployment and updates.
### Steps to instantiate a managed cluster of EC2 compute instances and deploy your image as a container running on the cluster
1. initialize Copilot application into the directory that has the application code.Copilot Application creates an empty application that consists of roles to administrate StackSets, ECR repositories, KMS keys, and S3 buckets. It also creates a local directory in your repository to hold configuration files for your application and services.
   do this by running the command
   `` copilot app init `` ![Screenshot (116)](https://github.com/user-attachments/assets/a665ecef-8a3c-4690-ba43-dd074ff563e5)
     
2. Next, Create ennvironment where application will run using the command ``copilot env init`` AWS Copilot provides a secure VPC, an Amazon ECS cluster, a Load Balancer, and all the other resources required by the application using the manifest.yaml to configure the environment. ![Screenshot (121)](https://github.com/user-attachments/assets/01da4850-1088-49d9-987e-299883c3235e)![Screenshot (122)](https://github.com/user-attachments/assets/73e1ff57-4bcb-4a2d-87da-e905d399ad99)
3. Deploy the Environment: This step is to deploy the environment and provision the services for the application. To deploy, enter ``copilot env deploy --name api``  
4. Create the monolith AWS Copilot Service: This step uses a Load-balanced web Service for the Internet-facing monolith. To create the monolithic service, enter ``copilot svc init`` and choose Load Balanced Web Service. When you run copilot svc init, Copilot will likely prompt you for information such as the service name, service type, and other configuration details specific to the service you are initializing. This command is typically used to set up a new service within your Copilot application, defining how it should be deployed, its dependencies, and any necessary configurations. ![Screenshot (129)](https://github.com/user-attachments/assets/72a54c0e-7073-42e5-a1a7-58c51029defb) then ``cd copilot/monolith`` to Change the directory to /copilot/monolith.
5. Deploy the monolith AWS Copilot Service: To deploy the monolith Service, enter ``copilot svc deploy --name monolith`` in the terminal. When you deploy the service, the container is built locally by Docker and pushed to your Elastic Container Registry. The service pulls the container from the registry and deploys it in the environment. The monolith application is running when the deployment is complete.![Screenshot (132)](https://github.com/user-attachments/assets/17a503ac-deea-41a5-b422-0694f797e729)  Access your service using the load balancer link

## Break the Monolith
The Node.js application will be broken into several interconnected services and then create an AWS Copilot Service for each microservice.
The final application architecture uses Amazon ECS and the Application Load Balancer (ALB).
![Screenshot 2025-06-24 071508](https://github.com/user-attachments/assets/b71cf68e-ef0a-4025-9c78-ecfd340834ee)
1. Create the Services: 

We will set up a new microservice named posts as part of a larger application called api, using AWS Copilot by running the command ``copilot svc init --app api --dockerfile ./3-microservices/services/posts/Dockerfile --name posts --svc-type "Load Balanced Web Service" `` . This step involves telling Copilot how to build and deploy the posts service using a specific Dockerfile, and configuring it as a Load Balanced Web Service.  




   
