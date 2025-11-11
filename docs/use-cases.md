# Use Case Documentation
---

## Use Case 1: User Login

**Actor:** Employee/Admin  
**Description:** User logs in using email and password.

### Main Flow
1. User enters credentials  
2. System validates  
3. System returns JWT token  
4. User is redirected to respective dashboard  

---

## Use Case 2: Add Asset

**Actor:** Admin  
**Description:** Admin adds new asset to inventory.

### Main Flow
1. Admin opens Add Asset page  
2. Fills in asset details  
3. System saves asset in database  
4. Inventory count updates  

---

## Use Case 3: Allocate Asset to Employee

**Actor:** Admin  
**Description:** Admin assigns an asset to an employee.

### Main Flow
1. Admin selects employee  
2. Admin selects asset  
3. Sets issue date (return date = NULL by default)  
4. System saves allocation  
5. Asset status changes to **assigned**  
6. Notify employee via email  

---

## Use Case 4: Submit Return Request

**Actor:** Employee  
**Description:** Employee raises a request to return an assigned asset.

### Main Flow
1. Employee selects assigned asset  
2. Clicks **"Return Request"**  
3. Selects reason:  
    - No longer required  
    - Asset got damaged  
4. Adds a short description  
5. Admin notified via email  

---

## Use Case 5: Approve Return Request

**Actor:** Admin  
**Description:** Admin approves a return request.

### Main Flow
1. Admin views pending requests  
2. Admin reviews reason and details  
3. Admin approves  
4. On Approval:  
    - If **No longer required** → asset marked **available**  
    - If **Asset is damaged** → asset marked **damaged** or **under_repair**  
5. Notify employee via email  

---

## Use Case 6: View Inventory Dashboard

**Actor:** Admin  
**Description:** Admin views analytics and summary metrics.

### Main Flow
1. Admin opens dashboard  
2. System fetches real-time statistics  
3. Graphs displayed (Optional)

---

## Use Case 7: Password Reset Flow

**Actor:** Employee/Admin  
**Description:** User resets password when they forget it.

### Main Flow
1. User clicks **"Forgot Password"**  
2. User enters registered email  
3. System verifies the email  
4. System generates a unique **password reset token**  
5. Token stored in **password_reset_token** table  
6. System emails the reset link  
7. User opens link and resets password  

---

## Use Case 8: Register / Create Employee

**Actor:** Admin  
**Description:** Admin creates a new employee in the system.

### Main Flow
1. Admin opens **Create Employee** page  
2. Admin enters employee details 
3. System validates the details  
4. System creates employee account  
5. Login details emailed to employee  

---

## Use Case 9: Notification / Communication

**Actor:** System (Automated), Admin, Employee  
**Description:** System sends email notifications for important events.

### Main Flow
1. Trigger event occurs:  
    - Asset allocated  
    - Return request submitted  
    - Return request approved or rejected  
    - Password reset requested  
    - Employee account created  
2. System generates notification  
3. System sends email to recipient  
 

---
