Provide your solution here:

# High Availability Trading System on AWS

## Overview
This system is designed to be:
- **Highly available and resilient** to failures with **automatic failover and redundancy.**
- **Scalable** to handle growing traffic and workload demands.
- **Cost-effective** by leveraging auto-scaling, reserved instances, and managed services.

---

## 1. Overview Diagram
The architecture consists of the following key components:  
- **VPC with Public and Private Subnets**
- **Auto Scaling Group (ASG) and Application Load Balancer (ALB)**
- **EC2 Instances with Docker Containers (Front-End and Back-End)**
- **RDS (Relational Database Service)**
- **S3 and CloudFront for Static Content Delivery**
- **CloudWatch for Monitoring and Logging**
- **NAT Gateway for Outbound Access from Private Subnet**
- **AWS CodeCommit and CodePipeline for CI/CD**

[Overview Diagram](https://drive.google.com/file/d/1BSlk9TENg0l7YhkNJNbdf6bHRK7J7UGl/view?usp=sharing)
---

## 2. Services and Roles

### VPC (Virtual Private Cloud)
- **Purpose:**
  - Provide a secure and isolated network environment with **high-speed communication** between services.  
  - Separation of public and private subnets for enhanced security and reduced latency.
- **Key Components:**
  - **Public Subnets:** Host the **Front-End EC2 Instances** and **ALB.**
  - **Private Subnets:** Host the **Back-End EC2 Instances** and **RDS.**
  - **NAT Gateway:** Allows instances in private subnets to access the internet for updates and patches.
  - **Security Groups and NACLs:** Control inbound and outbound traffic between subnets and instances.
- **Why:**
  - Ensures **low-latency communication** within the same VPC.
  - **Enhanced security** by isolating the database in a private subnet.

### Auto Scaling Group (ASG) and Application Load Balancer (ALB)
- **Purpose:**
  - **ASG:** Ensures **high availability and scalability** by automatically adding or removing EC2 instances based on demand.
  - **ALB:** Distributes incoming traffic across multiple instances to balance load and maintain availability.
- **Why:**
  - **Auto-scaling** to handle spikes in traffic.
  - **High availability** with **multi-AZ deployment** for fault tolerance.

### EC2 Instances with Docker Containers
- **Purpose:**
  - Host **Front-End and Back-End applications** using Docker for consistent and scalable deployment.
  - **Docker** ensures consistent environments and easy rollbacks during deployment.
- **Why:**
  - **Cost-effective** compared to fully managed container services.
  - **High control** over the underlying infrastructure and scaling.
- **Alternatives:**
  - **ECS (Elastic Container Service):** For serverless container management with EC2 or Fargate but at a higher cost
  - **EKS (Elastic Kubernetes Service):** For advanced container orchestration with Kubernetes.

### RDS (Relational Database Service)
- **Purpose:**
  - Manages the relational database with **automated backups, patching, and scaling.**
  - **Multi-AZ deployment** for high availability and automatic failover.
- **Why:**
  - **Managed service** reduces operational overhead.
  - Ensures **high availability and data durability.**
- **Alternatives:**
  - **Aurora:** For higher performance and lower latency but at a higher cost.
  - **DynamoDB:** For NoSQL use cases such as caching and session management.
  - **Database on EC2:** More control but requires manually handle backups, patching, and scaling; Risks of more downtime and data loss.

### S3 and CloudFront
- **Purpose:**
  - **S3:** Store and serve static content (images, scripts, stylesheets).
  - **CloudFront:** Global CDN to **reduce latency and improve content delivery speed.**
- **Why:**
  - **High availability and scalability** with low latency using CloudFront's edge locations.
  - **Cost optimization** by reducing origin requests and data transfer costs.

### CloudWatch
- **Purpose:**
  - **Monitoring and alerting** for infrastructure and application metrics.
  - **CloudWatch Agent** installed on instances for detailed system metrics and custom logs.
- **Why:**
  - **Integrated monitoring and alerting** with auto-scaling triggers.
  - **No maintenance overhead** since it's a fully managed service.
- **Alternatives:**
  - **Prometheus and Grafana / ELK Stack :** For more flexible monitoring and alerting with centralized log aggregation.

### Docker Hub
- **Purpose:**
  - Store and manage Docker images for consistent deployments.
- **Why:**
  - Ensures **reproducible builds** and faster deployments.
- **Alternatives:**
  - **Elastic Container Repository (ECR):** Integrates with ECS and EKS for seamless container deployments.
  - **GitHub Packages:** For public or third-party container image management.

### CI/CD Pipeline
- **Purpose:**
  - To ensure continuous integration and continuous deployment (CI/CD), the following components are recommended:
    - **AWS CodeCommit**:
      - Source repository for version control of application code.
      - **Alternatives:** GitHub, Bitbucket, or GitLab for third-party repositories with better integration and collaboration tools.
    - **AWS CodePipeline**:
      - Automates the build, test, and deployment process, ensuring consistent application delivery.
      - **Alternatives:** Jenkins, CircleCI, or GitHub Actions for flexible CI/CD workflows and multi-cloud deployments.
    - **AWS CodeBuild**:
      - Compiles source code, runs tests, and produces deployable artifacts.
    - **AWS CodeDeploy**:
      - Manages deployments to EC2 instances, ensuring zero-downtime with rolling updates.
---

## 3. Scaling Plans
### When the product grows beyond the current setup:
- **EKS (Elastic Kubernetes Service):**
  - Transition to **Kubernetes for better container orchestration.**
  - **Easier management** of clusters and more efficient scaling.
- **Aurora Database:**
  - **Seamless auto-scaling** and higher throughput for growing data demands.
- **Grafana and Prometheus / ELK Stack :**
  - **Advanced monitoring and alerting** with centralized log aggregation.
- **No-SQL database :**
  - **More features** open for more feature such as flexible schema design, horizontal scaling for high availability, and optimized storage for unstructured data like JSON or XML because of providing faster read and write performance
---
