# AWS-HA-Architecture
# AWS High Availability Web Architecture ☁️

A fault-tolerant, highly available web architecture deployed on Amazon Web Services (AWS) using Infrastructure as Code (IaC) via AWS CloudFormation. This project was developed as part of the DEPI AWS Cloud Security training program.

## 👥 Team Members
* **Rana Abdelsalam**
* **Heba Youssef**
* **Menna Gamal**

## 🏗️ Architecture Diagram
The following diagram is generated using Mermaid.js to illustrate our VPC structure and traffic flow:

```mermaid
graph LR
    Users((Internet Users)) --> IGW[Internet Gateway]
    IGW --> ALB{Application Load Balancer}
    
    subgraph VPC [VPC: 10.0.0.0/16]
        ALB
        
        subgraph AZ_A [Availability Zone A: us-east-1a]
            subgraph PUB_A [Public Subnet: 10.0.1.0/24]
                EC2_A[EC2: Team-Web-Server]
            end
            subgraph PRIV_A [Private Subnet: 10.0.3.0/24]
                Empty_A[...]
            end
        end
        
        subgraph AZ_B [Availability Zone B: us-east-1b]
            subgraph PUB_B [Public Subnet: 10.0.2.0/24]
                EC2_B[EC2: Team-Web-Server]
            end
            subgraph PRIV_B [Private Subnet: 10.0.4.0/24]
                Empty_B[...]
            end
        end
    end

🛠️ Infrastructure Components
Virtual Private Cloud (VPC): Custom network spanning two Availability Zones.

Public & Private Subnets: Structured for security and scalability.

Internet Gateway (IGW): Enables outbound and inbound internet access for public subnets.

Application Load Balancer (ALB): Distributes incoming HTTP traffic across the EC2 instances.

Auto Scaling Group (ASG): Dynamically scales the EC2 fleet (Min: 2, Max: 4) using a custom Launch Template.

Security Groups: Enforces the principle of least privilege (EC2 instances only accept traffic from the ALB).

🚀 Deployment Instructions
Clone this repository:

Bash
git clone [https://github.com/RanaAbdelsalamm/AWS-HA-Architecture.git](https://github.com/RanaAbdelsalamm/AWS-HA-Architecture.git)
Log in to the AWS Management Console and navigate to CloudFormation.

Select Create stack > With new resources (standard).

Upload the template.yaml file included in this repository.

Provide a Stack Name and click Next through the options, then click Submit.

Wait for the status to show CREATE_COMPLETE.

Navigate to the EC2 Console -> Load Balancers, copy the ALB DNS name, and paste it into your browser to view the live web server.

🛡️ Security Posture
ALB Security Group: Allows HTTP 80 from 0.0.0.0/0.

Web Server Security Group: Strictly allows HTTP 80 only from the ALB Security Group, preventing direct external access to the EC2 instances.
    
    ALB --> EC2_A
    ALB --> EC2_B
