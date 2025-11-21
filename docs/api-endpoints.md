# API Endpoints Documentation

## Authentication (`/auth`)

- `POST /auth/login` - User login with email and password, returns access and refresh tokens
- `POST /auth/password/reset-request` - Request password reset by email
- `POST /auth/password/reset` - Confirm password reset using token
- `POST /auth/refresh` - Refresh access token using refresh token

## Admin - Employee Management (`/admin/employees`)

- `POST /admin/employees` - Create a new employee (Admin only)
- `GET /admin/employees` - List all employees with pagination and search (Admin only)
- `GET /admin/employees/{user_id}` - Get employee details by ID (Admin only)
- `PUT /admin/employees/{user_id}` - Update employee information (Admin only)
- `DELETE /admin/employees/{user_id}` - Soft delete an employee (Admin only)

## Employee - Self Service (`/employees/me`)

- `GET /employees/me` - Get current authenticated user's profile
- `GET /employees/me/allocations` - Get current user's asset allocations
- `GET /employees/me/requests` - Get current user's return requests

## Asset Categories (`/categories`)

- `POST /categories` - Create a new asset category (Admin only)
- `GET /categories` - List all asset categories (Admin only)
- `PUT /categories/{id}` - Update asset category by ID (Admin only)

## Assets (`/assets`)

- `POST /assets` - Create a new asset (Admin only)
- `GET /assets` - List all assets (Admin only)
- `GET /assets/{id}` - Get asset details by ID
- `PUT /assets/{id}` - Update asset information (Admin only)
- `DELETE /assets/{id}` - Soft delete an asset (Admin only)
- `GET /assets/{id}/history` - Get asset history/audit trail
- `PUT /assets/{id}/status` - Update asset status (Admin only)
- `GET /assets/{asset_id}/allocations` - Get all allocations for a specific asset

## Allocations (`/allocations`)

- `POST /allocations` - Allocate an asset to an employee (Admin only)
- `GET /allocations` - List all allocations (Admin only)

## Return Requests (`/requests`)

- `POST /requests` - Create a return request for an allocated asset
- `GET /requests/pending` - List all pending return requests (Admin only)
- `POST /requests/{id}/approve` - Approve a return request (Admin only)

## Dashboard (`/dashboard`)

- `GET /dashboard` - Get asset dashboard summary with counts by status (Admin only)

## Root

- `GET /` - Welcome message and API status

