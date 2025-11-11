# **Database Schema Documentation**

This document describes the complete database schema for the Asset Allocation System, including all tables, fields, relationships, types, indexes, triggers, and views.

---

# **1. Tables and Fields**

# **1.1 User**

| Field         | Type               | Description |
|---------------|--------------------|-------------|
| id            | UUID PK            | User ID |
| email         | CITEXT UNIQUE      | User email |
| password_hash | TEXT               | Hashed password |
| full_name     | TEXT               | Full name |
| role          | user_role ENUM     | admin or employee |
| employee_code | TEXT UNIQUE        | Employee ID/code |
| phone         | TEXT               | Phone number |
| deleted_at    | TIMESTAMPTZ        | Soft delete timestamp |
| created_at    | TIMESTAMPTZ        | Creation timestamp |
| updated_at    | TIMESTAMPTZ        | Last update timestamp |


**Indexes**
- idx_user_role on (role)

---

# **1.2 password_reset_token**

| Field       | Type                 | Description |
|-------------|----------------------|-------------|
| id          | UUID PK              | Token ID |
| user_id     | UUID FK(User.id)     | Linked user |
| token       | TEXT UNIQUE          | Password reset token |
| expires_at  | TIMESTAMPTZ          | Expiration time |
| used_at     | TIMESTAMPTZ          | When token was used |
| created_at  | TIMESTAMPTZ          | Creation timestamp |


**Indexes**
- idx_password_reset_user on (user_id)

---

# **1.3 asset_category**

| Field       | Type          | Description |
|-------------|---------------|-------------|
| id          | SERIAL PK     | Category ID |
| name        | TEXT UNIQUE   | Category name |
| description | TEXT          | Description |
| created_at  | TIMESTAMPTZ   | Creation timestamp |
| updated_at  | TIMESTAMPTZ   | Last update timestamp |


---

# **1.4 asset**

| Field           | Type                  | Description |
|-----------------|------------------------|-------------|
| id              | UUID PK               | Asset ID |
| category_id     | INT FK(asset_category.id) | Category |
| name            | TEXT                  | Asset name |
| tag_code        | TEXT UNIQUE           | Tag/code identifier |
| serial_number   | TEXT UNIQUE           | Serial number |
| status          | asset_status ENUM     | Asset status |
| purchase_date   | DATE                  | Purchase date |
| purchase_cost   | NUMERIC(12,2)         | Cost |
| warranty_expiry | DATE                  | Warranty expiry |
| specs           | JSONB                 | Specifications |
| created_at      | TIMESTAMPTZ           | Creation timestamp |
| updated_at      | TIMESTAMPTZ           | Last update timestamp |
| deleted_at      | TIMESTAMPTZ           | Soft delete timestamp |


**Indexes**
- idx_asset_category  
- idx_asset_status

---
# **1.5 allocation**

| Field          | Type                      | Description |
|----------------|---------------------------|-------------|
| id             | UUID PK                  | Allocation ID |
| asset_id       | UUID FK(asset.id)        | Asset |
| employee_id    | UUID FK(User.id)         | Allocated to |
| allocated_by   | UUID FK(User.id)         | Allocated by |
| allocation_date| DATE                     | Allocation date |
| returned_at    | TIMESTAMPTZ              | When returned |
| notes          | TEXT                     | Extra notes |
| created_at     | TIMESTAMPTZ              | Creation timestamp |
| updated_at     | TIMESTAMPTZ              | Last update timestamp |
| deleted_at     | TIMESTAMPTZ              | Soft delete timestamp |


**Indexes**
- uq_open_allocation_per_asset (partial unique)  
- idx_alloc_employee_open (partial)

---

# **1.6 return_request**

| Field         | Type                       | Description |
|---------------|-----------------------------|-------------|
| id            | UUID PK                    | Request ID |
| allocation_id | UUID FK(allocation.id)     | Related allocation |
| requested_by  | UUID FK(User.id)           | Request raised by |
| description   | TEXT                       | Description/details |
| approved_at   | TIMESTAMPTZ                | Approval time (NULL = pending) |
| decision_note | TEXT                       | Admin note |
| created_at    | TIMESTAMPTZ                | Creation timestamp |
| updated_at    | TIMESTAMPTZ                | Last update timestamp |


**Indexes**
- idx_return_request_pending (partial)

**Constraint**
- uq_open_return_request UNIQUE(allocation_id)

---

# **1.7 asset_history**

| Field        | Type                           | Description |
|--------------|----------------------------------|-------------|
| id           | BIGSERIAL PK                    | History ID |
| asset_id     | UUID FK(asset.id)               | Related asset |
| user_id      | UUID FK(User.id)                | Performed by |
| event_type   | asset_history_event ENUM        | Event type |
| from_status  | asset_status ENUM               | Previous status |
| to_status    | asset_status ENUM               | New status |
| metadata     | JSONB                           | Extra data |
| occurred_at  | TIMESTAMPTZ                     | When it happened |


**Indexes**
- idx_asset_history_asset_time

---

# **2. ER Diagram**

![ER Diagram](media/asset_er_diagram.png)

---

# **3. Types (Enums)**

### **user_role**
Values:
- `admin`
- `employee`

**Why:** Defines authorization boundary clearly in the database.

---

### **asset_status**
Values:
- `available`
- `assigned`
- `under_repair`
- `damaged`

**Why:** Normalized state machine for asset lifecycle.

---

### **asset_history_event**
Values:
- `allocated`
- `returned`
- `status_changed`
- `repair_started`
- `repair_completed`
- `damaged`

**Why:** Compact, queryable set of event labels for audit logging and dashboards.

---

# **4. Indexes**

### **idx_user_role on `User(role)`**
**Why:** Fast role-based queries (e.g., list all employees for admin pages).

---

### **idx_password_reset_user on `password_reset_token(user_id)`**
**Why:** Quick lookup of the latest reset tokens for a particular user.

---

### **idx_asset_category on `asset(category_id)`**
**Why:** Speeds up asset listings filtered by category.

---

### **idx_asset_status on `asset(status)`**
**Why:** Dashboard widgets rely on instant status-based counts.

---

### **uq_open_allocation_per_asset (UNIQUE PARTIAL)**  
On: `allocation(asset_id)`  
Where: `returned_at IS NULL`

**Why:** Enforces “only one active allocation per asset” at the database level.

---

### **idx_alloc_employee_open (PARTIAL)**  
On: `allocation(employee_id)`  
Where: `returned_at IS NULL`

**Why:** Instant fetch of “assets currently allocated to an employee.”

---

### **idx_return_request_pending (PARTIAL)**  
On: `return_request(allocation_id)`  
Where: `approved_at IS NULL`

**Why:** Very fast retrieval of pending return requests.

---

### **idx_asset_history_asset_time**  
On: `asset_history(asset_id, occurred_at DESC)`

**Why:** Efficient retrieval of chronological asset activity logs.

---

# **5. Triggers**

### **set_updated_at()**
Function behavior:
- Sets `NEW.updated_at = now()` on every update.

Applied On:
- `User`
- `asset`
- `asset_category`
- `asset_allocation`
- `return_request`

**Why:** Ensures consistent automatic timestamping across all tables without backend code duplication.

---

# **6. Views**

## **6.1 current_allocations**
Shows all active (not returned) asset allocations, including asset & employee details.

**Why:** Simplifies dashboard queries for “who is holding what right now.”

---

## **6.2 asset_status_counts**
Returns counts of assets grouped by their `status`.

**Why:** Used to populate overview statistics on the dashboard.

---

## **6.3 pending_return_requests**
Lists all return requests where `approved_at IS NULL`.

**Why:** Powers the admin’s "Pending Approvals" screen with a single efficient query.
