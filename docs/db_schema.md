# **Database Schema Documentation**

This document describes the complete database schema for the Asset Allocation System, including all tables, fields, relationships, types, indexes, triggers, and views.

---

# **1. Tables and Fields**

## **1.1 User**
| Field          | Type         | Description |
|----------------|--------------|-------------|
| id             | UUID PK      | Primary key |
| email          | CITEXT UNIQUE| User login email |
| password_hash  | TEXT         | Hashed password |
| full_name      | TEXT         | Full name |
| role           | user_role    | admin / employee |
| deleted_at     | TIMESTAMPTZ  | Soft delete timestamp |
| created_at     | TIMESTAMPTZ  | Created timestamp |
| updated_at     | TIMESTAMPTZ  | Updated timestamp |

**Indexes**
- idx_user_role on (role)

---

## **1.2 employee_profile**
| Field          | Type     | Description |
|----------------|----------|-------------|
| user_id        | UUID PK/FK(User.id) | Linked to User |
| employee_code  | TEXT UNIQUE | Employee code |
| employee_name  | TEXT NOT NULL | Employee name |
| phone          | TEXT | Mobile number |

---

## **1.3 password_reset_token**
| Field        | Type         | Description |
|--------------|--------------|-------------|
| id           | UUID PK      | Token ID |
| user_id      | UUID FK(User.id) | User |
| token        | TEXT UNIQUE  | Reset token |
| expires_at   | TIMESTAMPTZ  | Expiry |
| used_at      | TIMESTAMPTZ  | When used |
| created_at   | TIMESTAMPTZ  | Creation |

**Indexes**
- idx_password_reset_user on (user_id)

---

## **1.4 asset_category**
| Field        | Type         | Description |
|--------------|--------------|-------------|
| id           | SERIAL PK    | Category ID |
| name         | TEXT UNIQUE  | Category name |
| description  | TEXT         | Description |
| created_at   | TIMESTAMPTZ  | Created |
| updated_at   | TIMESTAMPTZ  | Updated |

---

## **1.5 asset**
| Field          | Type            | Description |
|----------------|------------------|-------------|
| id             | UUID PK          | Asset ID |
| category_id    | INT FK(asset_category.id) | Category |
| name           | TEXT NOT NULL    | Asset name |
| tag_code       | TEXT UNIQUE      | Internal tag |
| serial_number  | TEXT UNIQUE      | Serial number |
| status         | asset_status     | Current status |
| purchase_date  | DATE             | Purchase date |
| purchase_cost  | NUMERIC(12,2)    | Cost |
| warranty_expiry| DATE             | Warranty |
| specs          | JSONB            | Specifications |
| created_at     | TIMESTAMPTZ      | Created |
| updated_at     | TIMESTAMPTZ      | Updated |

**Indexes**
- idx_asset_category  
- idx_asset_status

---

## **1.6 asset_allocation**
| Field               | Type     | Description |
|---------------------|----------|-------------|
| id                  | UUID PK  | Allocation ID |
| asset_id            | UUID FK(asset.id) | Target asset |
| employee_id         | UUID FK(User.id) | Employee |
| allocated_by        | UUID FK(User.id) | Admin |
| allocation_date     | DATE | Date of allocation |
| expected_return_date| DATE | Expected return |
| returned_at         | TIMESTAMPTZ | When returned |
| notes               | TEXT | Extra notes |
| created_at          | TIMESTAMPTZ | Created |
| updated_at          | TIMESTAMPTZ | Updated |

**Constraints**
- chk_alloc_self_assign (employee_id <> allocated_by)

**Indexes**
- uq_open_allocation_per_asset (partial unique)  
- idx_alloc_employee_open (partial)

---

## **1.7 return_request (Option B)**
| Field        | Type         | Description |
|--------------|--------------|-------------|
| id           | UUID PK      | Request ID |
| allocation_id| UUID FK(asset_allocation.id) | Allocation |
| requested_by | UUID FK(User.id) | Employee |
| description  | TEXT | Details |
| approved_at  | TIMESTAMPTZ | Approval timestamp (NULL = pending) |
| approval_note| TEXT | Admin note |
| created_at   | TIMESTAMPTZ | Created |
| updated_at   | TIMESTAMPTZ | Updated |

**Indexes**
- idx_return_request_pending (partial)

**Constraint**
- uq_open_return_request UNIQUE(allocation_id)

---

## **1.8 asset_history**
| Field        | Type                  | Description |
|--------------|------------------------|-------------|
| id           | BIGSERIAL PK          | History ID |
| asset_id     | UUID FK(asset.id)     | Asset |
| user_id      | UUID FK(User.id)      | Performed by |
| event_type   | asset_history_event   | Event name |
| from_status  | asset_status          | Previous status |
| to_status    | asset_status          | Updated status |
| metadata     | JSONB                 | Extra details |
| occurred_at  | TIMESTAMPTZ           | Timestamp |

**Indexes**
- idx_asset_history_asset_time

---

# **2. ER Diagram**

![ER Diagram](media/asset_allocation_system-2025-11-09-173834.png)

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
On: `asset_allocation(asset_id)`  
Where: `returned_at IS NULL`

**Why:** Enforces “only one active allocation per asset” at the database level.

---

### **idx_alloc_employee_open (PARTIAL)**  
On: `asset_allocation(employee_id)`  
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

## **6.1 vw_current_allocations**
Shows all active (not returned) asset allocations, including asset & employee details.

**Why:** Simplifies dashboard queries for “who is holding what right now.”

---

## **6.2 vw_asset_status_counts**
Returns counts of assets grouped by their `status`.

**Why:** Used to populate overview statistics on the dashboard.

---

## **6.3 vw_pending_return_requests**
Lists all return requests where `approved_at IS NULL`.

**Why:** Powers the admin’s "Pending Approvals" screen with a single efficient query.
