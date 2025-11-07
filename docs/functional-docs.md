# Functional Documentation
---
## User Roles

### Admin

- Manage inventory — perform CRUD operations on assets  
- Register Employee  
- Allocate assets to employees  
- Approve asset return requests  
- View employee profiles and assigned assets  
- Manage asset categories  
- View asset history & status  

### Employee

- View assigned assets  
- Submit return request (Reason: **No longer required** / **Asset is damaged**)  
- View request status  
- View asset history  
---
## **Functional Modules**

### 1. Authentication & Authorization

- Simple login/register  
- JWT authentication  
- Role-based access: Admin, Employee  
- Forgot/Reset Password  

### 2. Asset Categories

- Create, read, update, delete categories  
- Used to organize assets  

### 3. Asset Management (Inventory)

- Add assets with basic details  
- Edit/update assets  
- Change asset status: `available`, `assigned`, `under repair`, `damaged`  
- View all assets  

### 4. Asset Allocation (by Admin to Employee)

- Assign an asset to an employee  
- Set allocation date  
- Asset status changes to `assigned`  

### 5. Return Request (by the Employee)

- Employee submits request to return an asset  
- Must choose a reason:  
     * No longer required 
     * Asset is damaged
- Optional description  

### 6. Return Approval (by Admin)

- Admin views list of pending return requests  
- Admin reviews reason and details  
- On approval:  
  * If **No longer required** → asset becomes `available`  
  * If **Asset is damaged** → asset becomes `damaged` or `under_repair`  

### 7. Basic Admin Dashboard

- Total assets  
- Available assets  
- Assigned assets  
- Under repair  
- Damaged  
- Pending return requests  
- Register Employee  

### 8. Employee Dashboard

- List of assigned assets  
- Status of submitted return requests  

### 9. CI/CD

- Not decided yet, but most likely GitHub Actions  