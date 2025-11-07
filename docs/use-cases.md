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
3. Sets issue date & return date (default: Null)  
4. System saves allocation  
5. Asset status changes to **assigned**  
6. Notify employee via email  
---
## Use Case 4: Submit Return Request

**Actor:** Employee  
**Description:** Employee raises a request to return assigned asset.

### Main Flow

1. Employee selects assigned asset  
2. Clicks **"Return Request"**  
3. Selects Reason:  
   * No longer required  
   * Asset got damaged  
4. Adds a short description  
5. Admin is notified via email  
---
## Use Case 5: Approve Return Request

**Actor:** Admin  
**Description:** Admin approves a return request.

### Main Flow

1. Admin views pending requests  
2. Admin reviews reason and details  
3. Admin approves  
4. On Approval:  
   * If **No longer required** → asset marked **available**  
   * If **Asset is damaged** → asset marked **damaged** or **under_repair**  
5. Notify employee via email  
---
## Use Case 6: View Inventory Dashboard

**Actor:** Admin  
**Description:** Admin views analytics and summary metrics.

### Main Flow

1. Admin opens dashboard  
2. System fetches real-time statistics  
3. Graphs displayed *(optional — good for user experience)*  