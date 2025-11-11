# **Project Technology Stack**



---

##  Application Stack

### Frontend
| Component | Version/Tool | Purpose |
| :--- | :--- | :--- |
| **Framework** | **React 18** | JavaScript library for building the user interface. |
| **Build Tool** | **Vite 5.x** | Next-generation frontend tooling for fast development. |
| **UI Library** | **Material UI** | Comprehensive library for implementing Google's Material Design. |
| **Progressive Web App** | **PWA** | Enables offline capabilities and improved user experience. |

### Backend
| Component | Version/Tool | Purpose |
| :--- | :--- | :--- |
| **Language** | **Python 3** | Core language for the business logic and API. |
| **Web Framework** | **FastAPI 0.11+** | Modern, fast web framework for building APIs based on Python type hints. |
| **ASGI Server** | **Uvicorn** | High-performance ASGI server for running the FastAPI application. |
| **ORM** | **SQLAlchemy 2+** | Python SQL Toolkit and Object Relational Mapper. |
| **Database Migrations** | **Alembic 1.3+** | Database migration tool used with SQLAlchemy. |
| **Data Validation** | **Pydantic 2+** | Data validation and settings management using Python type hints. |

---

##  Database

| Component | Version/Tool | Purpose |
| :--- | :--- | :--- |
| **Primary Database** | **PostgreSQL 16.x** | Robust, open-source relational database. |

---

## Cloud Infrastructure (AWS Services)

| Service | Component | Purpose |
| :--- | :--- | :--- |
| **Compute** | **EC2** | Virtual servers for hosting the Frontend and Backend applications. |
| **Database** | **RDS PostgreSQL** | Managed relational database service for the primary database. |
| **DNS** | **Route 53** | Scalable cloud Domain Name System (DNS) web service. |
| **Networking** | **Elastic IP (EIP)** | Static, public IPv4 address for the EC2 instances. |
| **Security** | **ACM** (Optional) | Amazon Certificate Manager for managing TLS/SSL certificates. |
| **Email/Notifications** | **SNS/SES** | Used for sending emails (e.g., password reset tokens and asset allocation). |

---

##  Deployment and Testing

### Deployment
| Tool/Service | Purpose |
| :--- | :--- |
| **Containerization** | **Docker Hub** | Repository for storing and pulling Docker images. |
| **Orchestration** | **Docker-Compose** | Tool for defining and running multi-container Docker applications locally and on EC2. |
| **CI/CD** | **Github Actions** | Automation for Continuous Integration and Continuous Deployment workflows. |

### Testing
| Tool/Service | Purpose |
| :--- | :--- |
| **Unit Testing (Backend)** | **Pytest** | Python testing framework for writing simple, scalable unit tests. |
| **Unit Testing (Frontend)** | **React Testing Library** | Focused on testing application components as a user would. |
| **Static Analysis** | **SAST** (Tool - Not Decided Yet) | Static Application Security Testing: Analyzes source code for vulnerabilities without executing it. |
| **Dynamic Analysis** | **DAST** (Tool Not Decided Yet) | Dynamic Application Security Testing: Analyzes the running application for vulnerabilities. |