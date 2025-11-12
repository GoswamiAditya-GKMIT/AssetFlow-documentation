# **AWS Architecture Diagram**

![AWS Architecture Diagram](media/final-aws-arch.png)



A secure and CI/CD-enabled AWS setup for backend deployment with DNS routing, email integration, and automated updates.

---

##  Key Design Principles

| Feature | Purpose |
| :--- | :--- |
| **VPC Isolation** | Keeps compute and database within a secure private network. |
| **Public Subnet** | Hosts EC2; RDS is accessible only inside the same VPC. |
| **Security Groups** | Strict inbound/outbound control ensuring least privilege. |
| **CI/CD via GitHub Actions** | Automates testing and deployment to EC2. |
| **SES + Route 53** | Handles email delivery and DNS resolution. |

---

##  1. Entry Point (Amazon Route 53)

- Users access via a domain managed by **Route 53**.  
- **A Record** maps domain to EC2’s **Elastic IP**.  
- Routes traffic securely to the backend server.

---

##  2. Backend Server (EC2)

- Hosts core application and APIs.  
- Deployed in **Public Subnet**.  
- **Security Group:**  
  - Allows HTTP (80)/HTTPS (443) from Internet.  
  - Outbound access to RDS & SES.  
- **GitHub Actions** automates deployment via SSH/AWS CLI.

---

##  3. Database (Amazon RDS)

- **PostgreSQL RDS** runs within same VPC.  
- **Security Group:**  
  - Allows Port 5432 only from EC2 CIDR.  
  - No public internet access.  
- Ensures data integrity and isolation.

---

## 4. Email (Amazon SES)

- Backend uses **SES** for transactional emails (e.g., password reset).  
- Outbound-only communication — credentials stay private.

---

##  5. CI/CD Flow

1. Developer pushes code → GitHub Actions triggers pipeline.  
2. Builds and tests → Deploys directly to EC2 instance.  
3. Updated app auto-runs without manual intervention.

---

##  6. Security Summary

| Component | Rule | Purpose |
| :--- | :--- | :--- |
| **EC2** | Allow HTTP/HTTPS | Public access. |
| **RDS** | Port 5432 (VPC-only) | Backend-only DB access. |
| **SES** | Outbound only | Secure email delivery. |

---

##  Data Flow

**Users → Route 53 → EC2 → RDS → SES**  
**GitHub Actions → EC2 (CI/CD Deployment)**

---


