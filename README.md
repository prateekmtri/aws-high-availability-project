# Project: Resilient & Scalable Web Application Architecture on AWS ☁️

**[Technologies: ☁️ AWS | 🖥️ EC2 | ⚖️ ELB | 📜 AMI | 🛡️ VPC | 💾 EBS]**

---

### ## 📖 Table of Contents
1.  [**Project Summary**](#-project-summary)
2.  [**Architecture Diagram**](#-architecture-diagram)
3.  [**Core Concepts Demonstrated**](#-core-concepts-demonstrated)
4.  [**Step-by-Step Deployment Guide**](#-step-by-step-deployment-guide)
5.  [**How to Test the System**](#-how-to-test-the-system)
6.  [**Project Cleanup**](#-project-cleanup)
7.  [**Future Enhancements**](#-future-enhancements)

---

### ## 📝 Project Summary

This project demonstrates the deployment of a robust, highly available, and fault-tolerant web application using fundamental cloud-native principles on Amazon Web Services. The architecture is designed to ensure zero downtime by distributing traffic across multiple servers in different physical locations and automatically routing around failures. It serves as a practical implementation of core cloud infrastructure skills.

---

### ## 🏛️ Architecture Diagram

The infrastructure is logically isolated within a VPC and spread across two Availability Zones (AZs) for resilience. The Application Load Balancer serves as the single entry point, distributing traffic to the EC2 instances.

> **Pro Tip:** For a more professional look, you can create a graphical version of this diagram using a free tool like **diagrams.net** (draw.io) and embed the image in this README.

```
+--------------------------------------------------------------------------------+
| AWS Cloud                                                                      |
|                                                                                |
|  +--------------------------------------------------------------------------+  |
|  | Virtual Private Cloud (VPC)                                              |  |
|  |                                                                          |  |
|  |    +------------------------------------------------------------------+    |  |
|  |    | 🌐 Application Load Balancer (Internet-Facing)                 |    |  |
|  |    +-----------------|------------------------------------------------+    |  |
|  |                      |                                                     |  |
|  |  +-------------------|-------------------------------------------------+  |  |
|  |  | Target Group      |                                                 |  |  |
|  |  +-------------------|-------------------------------------------------+  |  |
|  |                      |                                                     |  |
|  |  +-------------------|-----------------------+-------------------------+  |  |
|  |  |                   |                       |                         |  |  |
|  |  |  Availability Zone A                  |  Availability Zone B        |  |  |
|  |  |                                       |                             |  |  |
|  |  |  +-----------------+                  |  +-----------------+          |  |  |
|  |  |  | 🖥️ EC2 Instance |◄----------------- |  | 🖥️ EC2 Instance |          |  |  |
|  |  |  |   (Web Server 1)|                  |  |   (Web Server 2)|          |  |  |
|  |  |  +-----------------+                  |  +-----------------+          |  |  |
|  |  |                                       |                             |  |  |
|  |  +---------------------------------------+-----------------------------+  |  |
|  |                                                                          |  |
|  +--------------------------------------------------------------------------+  |
|                                                                                |
+--------------------------------------------------------------------------------+

```

---

### ## ✨ Core Concepts Demonstrated

* **High Availability:** By deploying EC2 instances across two separate Availability Zones, the application is protected from a single point of failure at the data center level.
* **Fault Tolerance:** The Application Load Balancer's health checks continuously monitor the status of the instances. If an instance becomes unhealthy, the ELB automatically stops sending traffic to it, ensuring users are only served by healthy servers.
* **Scalability:** The use of a custom Amazon Machine Image (AMI) creates a "golden image" of the web server. This allows new instances to be launched rapidly and consistently, forming the foundation for horizontal scaling.
* **Infrastructure Security:** Security Groups are configured as stateful firewalls to strictly control inbound and outbound traffic to the EC2 instances, ensuring only necessary ports (like HTTP and SSH) are exposed.

---

### ## 🚀 Step-by-Step Deployment Guide

#### Phase I: Provisioning the Foundation (EC2 & Apache)
1.  **Launch Instance:** An Amazon Linux `t2.micro` EC2 instance was launched.
2.  **Configure Security Group:** A firewall rule was set up to allow inbound `HTTP` (Port 80) and `SSH` (Port 22) traffic.
3.  **Install Web Server:** Connected via SSH and installed the Apache (`httpd`) web server, enabled the service, and created a custom `index.html` page to identify it as `Server 1`.

#### Phase II: Creating a Scalable Blueprint (AMI)
1.  From the fully configured and running EC2 instance, a custom **Amazon Machine Image (AMI)** was created. This captures the state of the instance, including the OS, Apache configuration, and website files, into a reusable template.

#### Phase III: Horizontal Scaling (Launching Second Instance)
1.  A second EC2 instance was launched, but instead of using a default OS image, it was launched from the **custom AMI** created in Phase II.
2.  This new instance (`Server 2`) was a perfect clone and instantly operational. Its `index.html` file was slightly modified for testing purposes.

#### Phase IV: Implementing the Load Balancer (ELB)
1.  **Create Target Group:** A target group was created, and both EC2 instances were registered. Health checks were configured to monitor the instances' health.
2.  **Launch ELB:** An internet-facing **Application Load Balancer** was deployed and configured to listen for HTTP traffic on port 80 and forward it to the registered targets in the target group.

---

### ## ✅ How to Test the System
The success of the architecture is verified by accessing the public **DNS name** of the Application Load Balancer in a web browser. By repeatedly refreshing the page, the content can be seen switching between the pages served by "Server 1" and "Server 2," which confirms that the load balancing is working correctly.

---

### ## 🧹 Project Cleanup
To prevent ongoing AWS charges, all resources were terminated and deleted after project completion. The correct deletion order is: **ELB ➔ EC2 Instances ➔ Target Group ➔ AMI ➔ EBS Snapshots ➔ Security Group**.

---

### ## 🔮 Future Enhancements
This foundational project can be extended with more advanced AWS services:
* **Auto Scaling Group:** To automatically add or remove instances based on traffic load.
* **Amazon RDS:** To add a managed database for a dynamic application.
* **AWS Route 53:** To map a custom domain name (e.g., `www.my-cool-project.com`) to the Load Balancer.
* **AWS Certificate Manager (ACM):** To add a free SSL/TLS certificate and enable HTTPS.
