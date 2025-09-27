# AWS Cloud Project: Deploying a Highly Available & Fault-Tolerant Web Architecture

![AWS](https://img.shields.io/badge/AWS-%23232F3E.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) ![Amazon EC2](https://img.shields.io/badge/EC2-FF9900?style=for-the-badge&logo=amazon-ec2&logoColor=white) ![Amazon VPC](https://img.shields.io/badge/VPC-232F3E?style=for-the-badge&logo=amazon-vpc&logoColor=white) ![Shell Script](https://img.shields.io/badge/shell_script-%23121011.svg?style=for-the-badge&logo=gnu-bash&logoColor=white)

---

> 🚀 **This project is also documented in a post on my LinkedIn profile. You can view it and join the discussion here:**
>
> **[View My LinkedIn Post](https://www.linkedin.com/posts/prateek-mani-tripathi-51935a259_aws-cloudcomputing-devops-activity-7374456077733724160-Gtc9?utm_source=share&utm_medium=member_desktop&rcm=ACoAAD-XD2UB3Q_7K3wzLRZFKaD5o7TxIPOLoF8)**

---

### ## 📖 Project Summary

This repository contains a hands-on project that demonstrates the creation of a resilient, scalable, and highly available web application infrastructure on AWS. The architecture is meticulously designed to ensure zero downtime by leveraging multiple Availability Zones, automated health checks, and a load balancing system. This project serves as a practical showcase of fundamental cloud engineering skills and best practices for building fault-tolerant systems.

---

### ## 🏛️ Professional Architecture Diagram

The infrastructure is logically isolated within a custom VPC. An internet-facing Application Load Balancer distributes incoming HTTP traffic across two EC2 instances, each residing in a separate Availability Zone and protected by a shared Security Group.

```
+--------------------------------------------------------------------------------------------------+
|                                        AWS Cloud (ap-south-1)                                    |
|                                                                                                  |
|  +--------------------------------------------------------------------------------------------+  |
|  |                                  Virtual Private Cloud (VPC)                               |  |
|  |                                                                                            |  |
|  |    +------------------------------------------------------------------------------------+    |  |
|  |    |                            🌐 Application Load Balancer                            |    |  |
|  |    |                        (Listens on Port 80, Spans 2 AZs)                             |    |  |
|  |    +------------------------------------------|-------------------------------------------+    |  |
|  |                                               |                                                |  |
|  |  +--------------------------------------------|--------------------------------------------+  |  |
|  |  |                     🎯 Target Group with Health Checks                                  |  |  |
|  |  +--------------------------------------------|--------------------------------------------+  |  |
|  |                                               |                                                |  |
|  |     +-----------------------------------------+----------------------------------------+       |  |
|  |     |                                                                                  |       |  |
|  |  +--|---------------------------------------+  +----------------------------------------|--+    |  |
|  |  |  |      Availability Zone A              |  |      Availability Zone B               |  |    |  |
|  |  |  |                                       |  |                                        |  |    |  |
|  |  |  |  +---------------------------------+  |  |  +----------------------------------+  |  |    |  |
|  |  |  |  |    🛡️ Security Group           |  |  |  |    🛡️ Security Group            |  |  |    |  |
|  |  |  |  |  (Allows Port 80 & 22)          |  |  |  |  (Allows Port 80 & 22)           |  |  |    |  |
|  |  |  |  |                                 |  |  |  |                                  |  |  |    |  |
|  |  |  |  |  +---------------------------+  |  |  |  |  +----------------------------+  |  |  |    |  |
|  |  |  |  |  |     🖥️ EC2 Instance      |  |  |  |  |  |      🖥️ EC2 Instance       |  |  |  |    |  |
|  |  |  |  |  |      (Web Server 1)     |◄-+--|---|--+--|►     (Web Server 2)      |  |  |  |    |  |
|  |  |  |  |  +---------------------------+  |  |  |  |  +----------------------------+  |  |  |    |  |
|  |  |  |  +---------------------------------+  |  |  +----------------------------------+  |  |    |  |
|  |  +-------------------------------------------+  +------------------------------------------+  |  |
|  |                                                                                            |  |
|  +--------------------------------------------------------------------------------------------+  |
|                                                                                                  |
+--------------------------------------------------------------------------------------------------+
```

---

### ## ✨ Key Cloud Concepts Implemented

* **High Availability:** Deployed instances across two physically isolated Availability Zones to ensure the application remains operational even if one data center fails.
* **Fault Tolerance:** The Application Load Balancer automatically performs health checks and reroutes traffic away from any unhealthy or failing instances, ensuring seamless service continuity.
* **Scalability:** Created a custom Amazon Machine Image (AMI) from a fully configured server. This "golden image" enables rapid, consistent, and automated horizontal scaling to handle increased traffic.
* **Infrastructure Security:** Utilized Security Groups as a stateful firewall to enforce strict access rules, allowing only necessary HTTP and SSH traffic to the EC2 instances.

---

### ## 🚀 Step-by-Step Deployment Walkthrough

1.  **Foundation:** Launched an Amazon Linux EC2 instance, installed an Apache web server, and configured it with a custom webpage.
2.  **Blueprint:** Created a reusable server template (AMI) from this configured instance to serve as a blueprint for all future web servers.
3.  **Scaling Out:** Launched a second EC2 instance from the custom AMI in a different Availability Zone to build redundancy.
4.  **Traffic Management:** Deployed an Application Load Balancer (ELB) and configured it with a target group containing both instances. The ELB was set up to listen for web traffic and distribute it across the healthy targets.

---

### ## ✅ Final Result & Verification

The success of the architecture was verified by accessing the ELB's public DNS name. Refreshing the browser repeatedly showed the website content alternating between "Server 1" and "Server 2," confirming that the load balancing was functioning perfectly.

---

### ## 🧹 Project Cleanup

To adhere to best practices and avoid unnecessary costs, all AWS resources were decommissioned and deleted in the correct dependency order after the project's completion.

---

### ## 🔮 Potential Future Enhancements

* **Automation:** Implement an **Auto Scaling Group** to automatically scale the number of EC2 instances based on CPU utilization.
* **Database Tier:** Add a managed database layer using **Amazon RDS** for dynamic applications.
* **DNS & Security:** Use **AWS Route 53** to map a custom domain name and **AWS Certificate Manager (ACM)** to enable HTTPS.
* **CI/CD:** Build a CI/CD pipeline using **AWS CodePipeline** to automate application deployments.
