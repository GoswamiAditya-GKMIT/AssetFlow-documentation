# Authentication & Authorization

---

## 1. Overview

The Asset Management System uses:

- JWT Access & Refresh Tokens  
- Role-Based Access Control (RBAC)  
- Secure password reset  
- Backend permission checks  
- Email-based notifications  

These ensure secure login, controlled access, and safe user operations.

---

## 2. Authentication Architecture

### Components

| Component | Purpose |
|----------|---------|
| **React PWA** | Handles login UI and stores tokens |
| **FastAPI Backend** | Authenticates users and enforces permissions |
| **PostgreSQL DB** | Stores user data, roles, tokens |
| **Email Service** | Sends reset links and alerts |

---

## 3. Authentication Flows

### 3.1 Login

1. User submits email + password  
2. Frontend calls **POST /auth/login**  
3. Backend validates credentials  
4. Returns `access_token` + `refresh_token`  
5. Tokens stored in frontend  
6. Subsequent requests use:  
   `Authorization: Bearer <access_token>`

### 3.2 Refresh Token

1. Access token expires  
2. Frontend sends **POST /auth/refresh**  
3. Backend validates refresh token  
4. Issues new access token  

---

## 4. Password Reset Flow

Endpoints:  
- `POST /auth/password/reset-request`  
- `POST /auth/password/reset`  

Flow:

1. User requests password reset  
2. Backend generates token  
3. Token saved in DB  
4. Email sent with reset link  
5. User sets new password  
6. Token marked as used  

Security:  
- One-time token  
- 15-minute expiry  
- Mapped to specific user  

---

## 5. Authorization (RBAC)

Two roles:

- **admin**  
- **employee**  

Role stored as:  
`User.role ENUM('admin', 'employee')`

Admin has full management access; employees have limited operational access.

---

## 6. Service-to-Service Authentication

### Backend → Database
- Uses DB username/password  
- Only backend can connect  

### Frontend → Backend
- JWT-based  
- Always over HTTPS  
- No internal microservice tokens required  

---

## 7. System Interaction Flow

### React PWA → FastAPI
- Sends HTTP requests with JWT  
- JSON payloads  

### FastAPI → PostgreSQL
- Uses ORM/SQL queries  
- Credentials via environment variables  

---

## 8. Authorization Rules (API-Level)

### Employee
- Submit Return Request: `POST /return-requests`  

### Admin
- Approve Return Request: `POST /return-requests/{id}/approve`  
- View Dashboard: `GET /dashboard/summary`  

Each requires authentication + correct role.

---

## 9. Token Storage (Frontend)

- Access token → localStorage  
- Refresh token → localStorage or cookie  
- Both cleared on logout  

---

## 10. Future Enhancements

Potential improvements:

- OAuth2 or social login  
- Department-level roles  
- Single Sign-On (SSO)  

These are optional and not part of current scope.

