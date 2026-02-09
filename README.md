# Secure AWS Network Design for Internal Application Access


This repository contains the implementation of a secure AWS network architecture designed to host an internal web application that is accessible only to employees while remaining protected from the public internet.

The project focuses on subnet isolation, firewall configuration, controlled administrative access, and deployment of a containerized web application inside a private network.

---

## 🎯 Goal of the Project

To redesign an insecure cloud setup where servers were exposed directly to the internet and replace it with a properly segmented AWS architecture that follows cloud security best practices.

The implementation demonstrates:

- VPC and subnet design
- Controlled access using Security Groups and NACL
- Use of a Bastion Host for administration
- Use of NAT Gateway for private subnet internet access
- Deployment of a Docker-based web service in a private EC2 instance

---

## 🧩 Network Layout

            Internet
               |
        Internet Gateway
               |
    --------------------------
    |      Public Subnet     |
    |  Employee / Bastion EC2|
    --------------------------
               |
       HTTP / HTTPS / SSH
               |
    --------------------------
    |      Private Subnet    |
    |   EC2 running Docker   |
    --------------------------

- Public subnet simulates employee systems
- Private subnet hosts the internal application
- Direct internet access to the application server is blocked

---

## 🛠️ AWS Resources Used

| Resource | Purpose |
|----------|---------|
| VPC | Isolated virtual network |
| Public Subnet | Employee and admin access |
| Private Subnet | Application hosting |
| Internet Gateway | Public internet access |
| NAT Gateway | Outbound internet for private subnet |
| EC2 Instances | Bastion and application server |
| Security Groups | Instance-level firewall |
| Network ACL | Subnet-level firewall |
| Docker & Nginx | Web application deployment |

---

## 🔐 Access Control Strategy

### Initial Misconfiguration (Demonstration)

The application server was intentionally configured with an open security group allowing all traffic from everywhere to simulate a real-world mistake.

### Corrected Configuration

#### Public EC2

- SSH allowed only from administrator’s IP
- Acts as employee network and bastion host

#### Private EC2

- HTTP/HTTPS allowed only from public subnet (10.0.1.0/24)
- SSH allowed only from public subnet
- No public IP assigned

---

## 🌍 Routing Configuration

### Public Subnet
0.0.0.0/0 → Internet Gateway


### Private Subnet
0.0.0.0/0 → NAT Gateway


This ensures the private instance can install packages and access updates without being reachable from the internet.

---

## 🐳 Application Deployment

A simple Nginx container is deployed using Docker Compose on the private EC2 instance.

Verification from public subnet:

curl http://


This confirms internal accessibility while maintaining external isolation.

---

## 🧪 Validation Tests

| Scenario | Expected Outcome |
|----------|------------------|
| Internet → Private EC2 | Denied |
| Admin → Public EC2 | Allowed (SSH) |
| Public EC2 → Private EC2 | Allowed (HTTP/HTTPS/SSH) |
| Private EC2 → Internet | Allowed (via NAT) |

---

## 📘 Concepts Demonstrated

- Subnet segmentation in AWS
- Bastion host usage
- Practical difference between Security Groups and NACL
- Outbound internet access using NAT Gateway
- Principle of least privilege in network design

---

## 🚀 Steps Summary

1. Create VPC and subnets
2. Configure gateways and route tables
3. Launch EC2 instances
4. Apply insecure rule and document it
5. Correct the rules using best practices
6. Install Docker and deploy web server
7. Test connectivity between subnets


## 👨‍💻 Author

Rishikesh R 
AWS Cloud Network & Security Project
