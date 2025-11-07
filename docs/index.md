# Asset Allocation & Inventory Management System
                                                                    -  Aditya Giri Goswami
                                                               (Python Development Intern)
# **1. Project Overview**
##  -- Problem Statement

Corporate companies often distribute laptops, desktops, monitors, and other IT assets to employees. However, most organizations struggle with:

- **Poor tracking** of which employee has which asset  
- **Manual processes** like Excel sheets or emails  
- **No clear workflow** for handling returns  
- **Difficulty** in monitoring asset availability vs. assigned assets  
- **Inaccurate inventory counts** leading to over-purchasing or shortages  

These issues lead to operational inefficiencies, asset misuse, lack of accountability, and increased workload for IT/admin teams.



## -- Solution Overview

The **Asset Allocation & Inventory Management System** solves these problems by providing a centralized digital platform where:

- Admins can **manage inventory**, allocate items, and approve asset returns  
- Employees can **view assigned assets** and submit return requests  
- Asset availability and status are updated **in real time**  
- Dashboards offer **quick, clean insights** into inventory and allocations  
- Workflows become **faster, transparent, and easier to audit**

This system replaces manual tracking with a modern, structured solution that improves asset accountability, reduces administrative work, and ensures efficient asset utilization.

The **Asset Allocation & Inventory Management System** is designed to help corporate IT/admin teams efficiently manage company assets such as laptops, desktops, monitors, and accessories. The system tracks asset inventory, allocations to employees, returns, (damage reports - optional), and overall asset lifecycle.
## Tech Stack Used -
- Frontend - React + vite
- Backend - FastAPI
- Mobile - PWA (Progressive Web App)
- Database - Postgresql (SQLAlchemy as ORM)

---
# **2. Functional Documentation**
## **2.1 User Roles**
### 1. Admin
- Manage inventory  --- will perform CRUD operations on assets
- Register Employee
- Allocate assets to employees
- Approve asset return requests
- View all employee profiles and assigned assets
- Manage asset categories
- View asset history & status.
### 2. Employee
- View assigned assets
- Submit return request (with reason: No longer required or Asset is damaged)
- View request status
- View asset history
---
## **2.2 Functional Modules**
###  **1. Authentication & Authorization**
- Simple login/register
- JWT authentication
- Role-based access: Admin, Employee
- Forgot/Reset Password
###  **2. Asset Categories**
- Create, read, update, delete categories
- Used to organize assets
### **3. Asset Management (Inventory)**
- Add assets with basic details
- Edit/update assets
- Change asset status: `available`, `assigned`, `under repair` or `damaged`
- View all assets
###  **4. Asset Allocation (by Admin to employee)**
- Assign an asset to an employee
- Set allocation date
- Asset status changes to `assigned`
### **5. Return Request (by the Employee)**
- Employee submits request to return an asset
- Employee must choose a reason:
        - No longer required
        - Asset is damaged
- Optional description for context (especially for damaged case) 
###  **6. Return Approval (by Admin)**
- Admin views list of pending return requests
- Admin approves
- On approval:
            -- If No longer required → asset becomes available
            ---If Asset is damaged → asset becomes damaged or under_repair (based on admin choice)
###  **7. Basic Admin Dashboard**
- Total assets
- Available assets
- Assigned assets
- under repair
- damaged
- Pending return requests
- Register Employee
###  **8. Employee Dashboard**
- List of assigned assets
- Status of submitted return requests
###  **9. CI/CD**
- Not Decided yet but most probably will use github actions
---
# **3. Use Case Documentation**
##  Use Case 1: User Login
**Actor:** Employee/Admin
**Description:** User logs in using email and password.
**Main Flow:**
1. User enters credentials
2. System validates
3. System returns JWT token
4. User is redirected to respective dashboard
---
## Use Case 2: Add Asset
**Actor:** Admin
**Description:** Admin adds new asset to inventory.
**Main Flow:**
1. Admin opens Add Asset page
2. Fills in asset details
3. System saves asset in DB
4. Inventory count updates
---
## Use Case 3: Allocate Asset to Employee
**Actor:** Admin
**Description:** Admin assigns an asset to an employee.
**Main Flow:**
1. Admin selects employee
2. Admin selects asset
3. Sets issue date & return date(default Null)
4. System saves allocation entry
5. Asset status changes to **assigned**
6. Notify employee via Email
---
##  Use Case 4: Submit Return Request
**Actor:** Employee
**Description:** Employee raises a request to return assigned asset.
**Main Flow:**
1. Employee selects assigned asset
2. Clicks "Return Request"
3. Selects Reason: 
            - No longer required
            - Asset got damaged
4. Adds a short description
5. Admin is notified via Email
---
##  Use Case 5: Approve Return Request
**Actor:** Admin
**Description:** Admin approves a return request.
**Main Flow:**
1. Admin views pending requests
2. Admin reviews reason and details
3. Admin approves
4. On Approval - 
        - If No longer required → asset marked available
        - If Asset is damaged → asset marked damaged or under_repair
5. Notify employee via email
---
##  Use Case 7: View Inventory Dashboard
**Actor:** Admin
**Description:** Admin views analytics and summary metrics.
**Main Flow:**
1. Admin opens dashboard
2. System fetches real-time statistics
3. Graphs displayed (optional - good for scaling and User Experience)
---
#  Conclusion
This system is helpful for the admin in the corporate companies to manage their assets . The workflow and functionalities of this system is easy for the admin and employee to use