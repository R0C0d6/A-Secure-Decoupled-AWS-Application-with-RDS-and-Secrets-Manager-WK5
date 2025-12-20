#  Building a Secure, Decoupled AWS Web Application with RDS and Secrets Manager (From Scratch)

![Architecture Diagram](https://i.postimg.cc/W32LLrvB/ro-ARCH.png)

---

##  Project Overview

Modern cloud applications are not built as one big system. Instead, they are designed using **decoupled architecture**, where each component has a clear responsibility, is securely isolated, and can scale independently.
This project demonstrates how to build a **secure, real-world AWS architecture from scratch**, starting with networking and ending with a fully working web application connected to a managed database - the same way enterprise teams do it.

The goal of this project was to:
Understand AWS networking fundamentals
Build a custom Virtual Private Cloud (VPC)
Securely deploy a web application
Integrate a managed database (Amazon RDS)
Protect credentials using AWS Secrets Manager
Perform full CRUD (Create, Read, Update, Delete) operations
Apply AWS best practices for security and architecture

This project was intentionally built step by step, without shortcuts, to deeply understand why each component exists and how they work together.

---

## 🎯 Project Objectives

The main objectives of this project were:
  Build a **custom AWS VPC** from scratch
  Create **public and private subnets**
  Control internet access using **route tables and an Internet Gateway**
  Deploy a **web application on EC2** in a public subnet
  Deploy an **Amazon RDS MySQL database** in a private subnet
  Secure communication using **security groups**
  Store database credentials securely using **AWS Secrets Manager**
  Connect the web application to the database securely
  Perform and verify **CRUD operations**
  Demonstrate a **production-style cloud architecture**

---

## Key Architectural Concept: Decoupling

Before building anything, it’s important to understand the idea of **decoupling**.
In this project:
  The **application** does not store data itself
  The **database** is separate and private
  Credentials are not stored in code
  Each component communicates only through approved paths

This approach improves, Security, Scalability, Maintainability, Fault isolation. Decoupling is a core principle used in **enterprise cloud systems**.

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

If you're comfortable, proceed with the VPC & More Option (My Approach)
Name VPC
CIDR: 10.0.0.0/16

![VPC Creation](https://i.postimg.cc/W32LLrvB/ro-ARCH.png)


No IPV6 CIDR block
Default Tenancy
2 AZs
2 Public Subnets | 2 Private Subnets
Nat Gateway
![VPC Creation](https://i.postimg.cc/hGwnxhR4/Screenshot-2025-12-17-211657.png)


Leave everything else as default. 
Click create VPC (Other essential components will be created. IGW, RT, EIP, etc)
![VPC Creation](https://i.postimg.cc/0N9M3Wz6/Screenshot-2025-12-17-211936.png)


## 🖥️ Step 4: Deploying the Web Application (EC2)
An EC2 instance was launched to host the web application.

### EC2 Configuration
* Amazon Linux AMI
  ![VPC Creation](https://i.postimg.cc/QtHp44zV/Screenshot-2025-12-17-215600.png)

* Instance type: `t2.micro` (Free Tier)
![VPC Creation](https://i.postimg.cc/PJ1v9BQC/Screenshot-2025-12-17-215738.png)

* Select Your Created VPC
* Launch in the **public subnet**
* Select/Pick your security group (rules will be communicated later)
* Assigned a public IP address
![VPC Creation](https://i.postimg.cc/D05nS6v3/Screenshot-2025-12-17-215935.png)
Launch Instance

### Web Server Setup
The EC2 instance was also configured with:
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
* 
![VPC Creation](https://i.postimg.cc/c4Q5BCYk/Screenshot-2025-12-17-232259.png)

Outbound rules:
* Allowed all traffic (default)

### Database Security Group
Inbound rules:
* MySQL (port 3306)
* Source: **Web App Security Group only**
![DSG](https://i.postimg.cc/j5ckYFqb/Screenshot-2025-12-17-224500.png) 
![DSG](https://i.postimg.cc/fWYv6FLm/Screenshot-2025-12-17-232347.png)

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
![RDS](https://i.postimg.cc/d0d7Lbg9/Screenshot-2025-12-17-220811.png)

* Use Free tier to avoid incurring cost
![RDS](https://i.postimg.cc/90wshBDW/Screenshot-2025-12-17-220839.png)

* Use Secrets Manager to Manage Credentials
![RDS](https://i.postimg.cc/CLGSnFK7/Screenshot-2025-12-17-220915.png)

* Use t2 or t3 micro instance type for the DB.
* Use GP3 storage for low cost storage.
![RDS](https://i.postimg.cc/PJMVhFtL/Screenshot-2025-12-17-221109.png)

* Deployed in the **private subnet**
* Public access: Disabled

![RDS](https://i.postimg.cc/XYtLM0wx/Screenshot-2025-12-17-221203.png)

* Create New DB Subnet Group
![RDS](https://i.postimg.cc/jqQRLx2q/Screenshot-2025-12-17-223535.png)
![RDS](https://i.postimg.cc/KjJXQY3w/Screenshot-2025-12-17-223554.png)

* Connect to the database security group
![RDS](https://i.postimg.cc/8cydZ8pY/Screenshot-2025-12-17-221314.png)

Launch Database:
![RDS](https://i.postimg.cc/Rhf6nj30/Screenshot-2025-12-17-221931.png)

This ensures the database is Secure, Highly available, Not exposed to the internet

---


## 🔑 Step 8: Storing Credentials with AWS Secrets Manager

Hardcoding passwords is insecure. To solve this:
### AWS Secrets Manager was used to:
* Store credentials securely
* Encrypt secrets automatically
* Allow controlled access via IAM

From the Secrets Manager portal, choose secret type(rds) and configure secret by setting the name and configuring rotation
![Secrets Manager](https://i.postimg.cc/qMN0Q8Qt/Screenshot-2025-12-17-233656.png)
![Secrets Manager](https://i.postimg.cc/k4j3v4bp/Screenshot-2025-12-17-233744.png)
Review and Create

######################################################################################################################################

Again from the Secrets Manager portal, open up the rds secret.
![Secrets Manager](https://i.postimg.cc/8cBv02Tq/Screenshot-2025-12-17-233445.png)


Retrieve Secret Value
![Secrets Manager](https://i.postimg.cc/xTzhcgtp/Screenshot-2025-12-17-233518.png)
This allowed the application to retrieve credentials **securely at runtime**.

---


## 🗃️ Step 7: Creating and connecting the Application
SSH into the Wep App ( ssh -i "your key pair" ec2@your public ip )
![SSH](https://i.postimg.cc/hvTFW0wB/Screenshot-2025-12-17-235335.png)

Install SQL or mariadb. e.g.(sudo yum instal mariadb105 -y)
![SQL](https://i.postimg.cc/WbYmcPGv/Screenshot-2025-12-18-001938.png)

Connect to RDS from the EC2 instance: ( mariadb -h "rds domain" -u "username" -p )
You will receive a prompt to enter your password. Use the secret key you retrieved from Secrets Manager
![Connect to RDS](https://i.postimg.cc/Gpwyb39J/Screenshot-2025-12-18-004419.png)
You will receive a welcome message.

The web application:
1. Requests credentials from Secrets Manager
2. Uses those credentials to connect to RDS
3. Executes SQL queries securely

Use Show Databases to test connection to rds and show the Databases available in there.
![Connect to RDS](https://i.postimg.cc/nzLcJNK6/Screenshot-2025-12-18-145428.png)


* A database named `appdb` was created for our operation:
  ( CREATE DATABASE appdb; ).
  (USE appdb;) to switch to using the appdb database we created for our CRUD operations.

## Performing CRUD Operations. CRUD operations were tested to validate full functionality.
### Create
Tables were created inside the database
  Example table: `users` . 
  ![Connect to RDS](https://i.postimg.cc/4xZY0LMg/Screenshot-2025-12-18-145513.png)

This step verified that:
* Network connectivity was correct
* Security groups were properly configured
* EC2 could communicate with RDS securely



### Read ( SHOW TABLES; , SELECT * FROM USERS; )
* Query stored data

### Update (INSERT INTO users..., UDPDATE USERS...)
* Modify existing records safely using conditions
![Connect to RDS](https://i.postimg.cc/D0g9wz20/Screenshot-2025-12-18-145545.png)
![Connect to RDS](https://i.postimg.cc/G3QVYHCH/Screenshot-2025-12-18-145604.png)


### Delete
* Remove records selectively
![Connect to RDS](https://i.postimg.cc/J4YF6vtZ/Screenshot-2025-12-18-145821.png)
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
