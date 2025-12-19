
# 🚀 Building a Secure, Decoupled AWS Web Application with RDS and Secrets Manager (From Scratch)

## A Beginner-Friendly, End-to-End Cloud Architecture Project

---

## 📌 Project Overview

Modern cloud applications are not built as one big system. Instead, they are designed using **decoupled architecture**, where each component has a clear responsibility, is securely isolated, and can scale independently.

This project demonstrates how to build a **secure, real-world AWS architecture from scratch**, starting with networking and ending with a fully working web application connected to a managed database — **the same way enterprise teams do it**.

The goal of this project was to:

* Understand AWS networking fundamentals
* Build a custom Virtual Private Cloud (VPC)
* Securely deploy a web application
* Integrate a managed database (Amazon RDS)
* Protect credentials using AWS Secrets Manager
* Perform full CRUD (Create, Read, Update, Delete) operations
* Apply AWS best practices for security and architecture

This project was intentionally built step by step, without shortcuts, to deeply understand **why each component exists and how they work together**.

---

## 🎯 Project Objectives

The main objectives of this project were:

* Build a **custom AWS VPC** from scratch
* Create **public and private subnets**
* Control internet access using **route tables and an Internet Gateway**
* Deploy a **web application on EC2** in a public subnet
* Deploy an **Amazon RDS MySQL database** in a private subnet
* Secure communication using **security groups**
* Store database credentials securely using **AWS Secrets Manager**
* Connect the web application to the database securely
* Perform and verify **CRUD operations**
* Demonstrate a **production-style cloud architecture**

---

## 🧠 Key Architectural Concept: Decoupling

Before building anything, it’s important to understand the idea of **decoupling**.

In this project:

* The **web application** does not store data itself
* The **database** is separate and private
* Credentials are not stored in code
* Each component communicates only through approved paths

This approach improves:

* Security
* Scalability
* Maintainability
* Fault isolation

Decoupling is a core principle used in **enterprise cloud systems**.

---

## 🏗️ Architecture Overview

The final architecture consists of:

* **AWS VPC** – Isolated private network
* **Public Subnet** – Hosts the EC2 web server
* **Private Subnet** – Hosts the RDS database
* **Internet Gateway** – Allows public internet access to the web app
* **Route Tables** – Control traffic flow
* **Security Groups** – Act as firewalls
* **EC2 Instance** – Runs the web application
* **Amazon RDS (MySQL)** – Managed relational database
* **AWS Secrets Manager** – Secure storage for credentials

The database is **never exposed to the internet**, and only the web application can communicate with it.

---

## 🌐 Step 1: Creating the Virtual Private Cloud (VPC)

The first step was creating a **custom VPC**, which acts as a private cloud network.

### Why a Custom VPC?

Using a custom VPC gives full control over:

* IP addressing
* Subnet isolation
* Security boundaries
* Routing behavior

### VPC Configuration

* IPv4 CIDR block: `10.0.0.0/16`
* Allows thousands of private IP addresses
* Provides room for future expansion

This VPC serves as the foundation for everything else in the project.

---

## 🧱 Step 2: Creating Public and Private Subnets

Inside the VPC, two subnets were created:

### Public Subnet

* CIDR: `10.0.1.0/24`
* Hosts the EC2 web server
* Has access to the internet
* Used for resources that must be publicly reachable

### Private Subnet

* CIDR: `10.0.2.0/24`
* Hosts the RDS database
* No direct internet access
* Used for sensitive resources

This separation ensures **network isolation**, a core AWS security best practice.

---

## 🌍 Step 3: Internet Gateway and Routing

To allow internet access:

### Internet Gateway (IGW)

* Attached to the VPC
* Enables communication between the public subnet and the internet

### Route Tables

Two route tables were configured:

#### Public Route Table

* Route: `0.0.0.0/0 → Internet Gateway`
* Associated with the public subnet

#### Private Route Table

* No internet route
* Associated with the private subnet

This ensures:

* Web servers are accessible
* Databases remain private and protected

---

## 🖥️ Step 4: Deploying the Web Application (EC2)

An EC2 instance was launched to host the web application.

### EC2 Configuration

* Amazon Linux AMI
* Instance type: `t2.micro` (Free Tier)
* Launched in the **public subnet**
* Assigned a public IP address

### Web Server Setup

The EC2 instance was configured with:

* Apache (httpd)
* Basic web service
* Enabled on startup

This EC2 instance represents the **frontend or application layer** of the system.

---

## 🔐 Step 5: Securing Access with Security Groups

Security groups act as **virtual firewalls**.

### Web Application Security Group

Inbound rules:

* HTTP (port 80) – from anywhere
* SSH (port 22) – restricted to a trusted IP

Outbound rules:

* Allowed all traffic (default)

### Database Security Group

Inbound rules:

* MySQL (port 3306)
* Source: **Web App Security Group only**

This configuration ensures:

* Only the web app can talk to the database
* No public or external database access

---

## 🗄️ Step 6: Deploying Amazon RDS (MySQL)

A managed MySQL database was created using Amazon RDS.

### Why RDS?

* AWS handles backups
* Automatic patching
* High reliability
* No manual database management

### RDS Configuration

* Engine: MySQL
* Deployed in the **private subnet**
* Public access: Disabled
* Connected to the database security group

This ensures the database is:

* Secure
* Highly available
* Not exposed to the internet

---

## 🗃️ Step 7: Creating the Application Database

Once connected to RDS from the EC2 instance:

* A database named `appdb` was created
* Tables were created inside the database
* Example table: `users`

This step verified that:

* Network connectivity was correct
* Security groups were properly configured
* EC2 could communicate with RDS securely

---

## 🔑 Step 8: Storing Credentials with AWS Secrets Manager

Hardcoding passwords is insecure. To solve this:

### AWS Secrets Manager was used to:

* Store database credentials securely
* Encrypt secrets automatically
* Allow controlled access via IAM

The secret stored included:

* Database username
* Database password
* Database endpoint
* Database name

This allowed the application to retrieve credentials **securely at runtime**.

---

## 🔗 Step 9: Connecting the Web App to the Database

The web application:

1. Requests credentials from Secrets Manager
2. Uses those credentials to connect to RDS
3. Executes SQL queries securely

This design:

* Removes passwords from code
* Follows Zero Trust principles
* Matches real production systems

---

## 🧪 Step 10: Performing CRUD Operations

CRUD operations were tested to validate full functionality.

### Create

* Insert new records into the database

### Read

* Query stored data

### Update

* Modify existing records safely using conditions

### Delete

* Remove records selectively

All operations were verified, and data persistence was confirmed.

---

## 📸 Evidence and Validation

The following evidence was collected:

* VPC and subnet configurations
* Route table associations
* EC2 instance details
* RDS instance configuration
* Secrets Manager secret view
* Database output showing CRUD operations

These screenshots demonstrate that the system works end-to-end.

---

## 🔐 Security Best Practices Applied

This project follows AWS best practices:

* Principle of least privilege
* Network isolation
* No public database access
* Secure credential storage
* Decoupled architecture
* Managed services usage

---

## 🚀 Skills Demonstrated

This project demonstrates hands-on skills in:

* AWS Networking (VPC, Subnets, Routing)
* Cloud Security Design
* EC2 and RDS integration
* Secrets Manager
* MySQL database management
* Real-world cloud architecture
* Troubleshooting and debugging

---

## 🎓 Final Thoughts

This project is more than a tutorial — it is a **real cloud system** built from the ground up.
It demonstrates not just how to use AWS services, but **why they are used**, **how they connect**, and **how to secure them properly**.

The architecture, security controls, and design decisions align with what is expected in **Cloud Engineering, DevOps, and Solutions Architect roles**.

Building this project provided a strong foundation in cloud infrastructure, security, and application design — and serves as a solid portfolio piece for demonstrating real-world AWS capability.

---

### 🔗 If you’re learning AWS:

Start small. Build from scratch. Understand every piece.

That’s how real engineers are made.
