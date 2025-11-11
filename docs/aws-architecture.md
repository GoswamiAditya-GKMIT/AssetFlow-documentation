# **AWS Architecture Diagram**

![AWS Architecture Diagram](media/aws-architect.png)


## Key Design Principles

| Feature | Purpose |
| :--- | :--- |
| **VPC Separation** | Keeps all resources isolated within a private network. |
| **Public/Private Subnets** | **Public:** Frontend & Backend. **Private:** RDS (no internet exposure). |
| **Security Groups** | Stateful firewalls controlling which sources and ports can access components. |
| **Least Privilege Access** | Each tier only receives the minimum required inbound connections. |

---

## 1. Entry Point and DNS Resolution (Route 53)

- Users access the application via its domain.
- **Route 53** resolves the domain to the **Frontend EC2’s Elastic IP**.
- Traffic is routed directly to the frontend server’s public endpoint.

---

## 2. Frontend Tier (Public Subnet)

- Handles all user-facing requests.
- Accessible via **Elastic IP**.
- **Security Group:** Allows only **HTTP (80)** and **HTTPS (443)** from the public internet.
- Sends required API requests to the backend server.

---

## 3. Backend Tier (Public Subnet)

- Processes application logic and API requests.
- Communicates only with the frontend.
- **Security Group:** Allows **HTTPS (443)** exclusively from the **Frontend SG**.
- Executes secure queries to the database.

---

## 4. Database Tier (Private Subnet)

- **RDS PostgreSQL** is fully isolated in a private subnet.
- **Security Group:** Allows **Port 5432** only from the **Backend SG**.
- Backend sends and receives data securely.

---

## 5. Email Service Integration

- **SES** is used for outgoing transactional emails like password resets and notifications.
- Only the backend interacts with SES.

