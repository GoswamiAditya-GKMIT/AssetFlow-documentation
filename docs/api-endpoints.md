# API Endpoints

---

## Authentication

- **POST** `/auth/login`  
  User login and token issue

- **POST** `/auth/refresh`  
  Generate new access token

- **POST** `/auth/password/reset-request`  
  Send password reset email

- **POST** `/auth/password/reset`  
  Reset password using token

- **GET** `/auth/me`  
  Get current user profile

---

## Admin – Employees

- **POST** `/admin/employees`  
  Create a new employee

- **GET** `/admin/employees`  
  List all employees

- **GET** `/admin/employees/{id}`  
  Get employee details

- **PUT** `/admin/employees/{id}`  
  Update employee information

- **DELETE** `/admin/employees/{id}`  
  Soft delete employee account

---

## Asset Categories

- **POST** `/asset-categories`  
  Create new asset category

- **GET** `/asset-categories`  
  List all categories

- **PUT** `/asset-categories/{id}`  
  Update category details

---

## Assets

- **POST** `/assets`  
  Create new asset record

- **GET** `/assets`  
  List all assets

- **GET** `/assets/{id}`  
  Get asset details

- **PUT** `/assets/{id}`  
  Update asset information

- **DELETE** `/assets/{id}`  
  Soft delete an asset

- **GET** `/assets/{id}/history`  
  Fetch asset history logs

- **PUT** `/assets/{id}/status`  
  Update asset status field

---

## Allocations

- **POST** `/allocations`  
  Assign asset to employee

- **GET** `/allocations/current?employee_id={id}`  
  Get current allocations

- **GET** `/allocations/by-asset/{asset_id}`  
  Get allocation for asset

---

## Return Requests

- **POST** `/return-requests`  
  Submit return request

- **GET** `/return-requests/my`  
  List employee requests

- **GET** `/return-requests/pending`  
  Admin pending approvals

- **POST** `/return-requests/{id}/approve`  
  Approve return request

---
