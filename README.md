````markdown
# Supply Chain Management System (SCMS)

A full-stack Supply Chain Management System developed using React, TypeScript, Spring Boot, PostgreSQL, and WebSocket technology. The system provides centralized management of Stores, Employees, Materials, Suppliers, Manufacturers, Notifications, and Realtime Stock Indent communication.

---

## Features

### Authentication
- User Login
- Captcha Verification
- Session Management using Local Storage
- Role-based User Access

### Store Management
- Add Store
- View Stores
- Update Store
- Delete Store
- Search and Filter Stores

### Employee Management
- Add Employee
- View Employee Details
- Update Employee Information
- Delete Employee
- Store-wise Employee Mapping

### Material Management
- Add Material
- Update Material
- Delete Material
- Stock Monitoring

### Supplier Management
- Supplier Registration
- Supplier CRUD Operations

### Manufacturer Management
- Manufacturer Registration
- Manufacturer CRUD Operations

### Notification System
- Realtime Notifications
- User-Specific Notification Delivery

### Indent Management
- Create Indent Requests
- Approve Indents
- Reject Indents
- Realtime Status Updates
- User-to-User Communication

### Online User Tracking
- Realtime Online User Monitoring
- Live User Status Updates

---

# Technology Stack

## Frontend

- React
- TypeScript
- Vite
- Tailwind CSS
- STOMP Client
- SockJS

## Backend

- Spring Boot
- Spring Web
- Spring Data JPA
- Spring WebSocket
- Hibernate

## Database

- PostgreSQL

## Realtime Communication

- WebSocket
- STOMP Protocol

---

# System Architecture

```text
                PostgreSQL
                     ▲
                     │
              Repository Layer
                     ▲
                     │
                Service Layer
                     ▲
                     │
              Controller Layer
                     ▲
                     │
        --------------------------------
        │                              │
        │                              │
     REST APIs                   WebSocket
        │                              │
        ▼                              ▼

React Frontend            Realtime Communication

        ▲                              ▲
        │                              │
     CRUD APIs                 Notifications
                               Online Users
                               Indents
```

---

# Project Modules

## 1. User Module

Responsible for:

- Login Authentication
- Captcha Validation
- User Management

---

## 2. Store Module

Stores information related to:

- Store Code
- Store Name
- Location
- Store Type
- Manager Details
- Contact Information

---

## 3. Employee Module

Stores information related to:

- Employee Code
- Employee Name
- Designation
- Department
- Salary
- Joining Date
- Store Assignment

---

## 4. Material Module

Stores information related to:

- Material Code
- Material Name
- Category
- Unit
- Quantity
- Price

---

## 5. Supplier Module

Stores information related to:

- Supplier Code
- Supplier Name
- Contact Person
- GST Number
- Address

---

## 6. Manufacturer Module

Stores information related to:

- Manufacturer Code
- Manufacturer Name
- Contact Information
- Website

---

## 7. Notification Module

Provides:

- Realtime Notifications
- User-Specific Notification Delivery
- Read/Unread Tracking

---

## 8. Indent Module

Provides:

- Realtime Indent Creation
- Approval Workflow
- Rejection Workflow
- Status Tracking

---

# Database Schema

## Users

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    username VARCHAR(100),
    password VARCHAR(255),
    role VARCHAR(50)
);
```

## Stores

```sql
CREATE TABLE stores (
    store_id BIGSERIAL PRIMARY KEY,
    store_code VARCHAR(50),
    store_name VARCHAR(255),
    location VARCHAR(255),
    store_type VARCHAR(50),
    manager_name VARCHAR(255),
    contact_number VARCHAR(20),
    email VARCHAR(100),
    status VARCHAR(20)
);
```

## Employees

```sql
CREATE TABLE employees (
    employee_id BIGSERIAL PRIMARY KEY,
    employee_code VARCHAR(50),
    employee_name VARCHAR(255),
    designation VARCHAR(100),
    department VARCHAR(100),
    phone VARCHAR(20),
    email VARCHAR(100),
    address TEXT,
    joining_date DATE,
    salary DECIMAL(12,2),
    store_id BIGINT,
    status VARCHAR(20)
);
```

## Materials

```sql
CREATE TABLE materials (
    material_id BIGSERIAL PRIMARY KEY,
    material_code VARCHAR(50),
    material_name VARCHAR(255),
    category VARCHAR(100),
    unit VARCHAR(50),
    price DECIMAL(12,2),
    stock_quantity INTEGER,
    status VARCHAR(20)
);
```

## Suppliers

```sql
CREATE TABLE suppliers (
    supplier_id BIGSERIAL PRIMARY KEY,
    supplier_code VARCHAR(50),
    supplier_name VARCHAR(255),
    contact_person VARCHAR(255),
    phone VARCHAR(20),
    email VARCHAR(100),
    address TEXT,
    gst_number VARCHAR(50)
);
```

## Manufacturers

```sql
CREATE TABLE manufacturers (
    manufacturer_id BIGSERIAL PRIMARY KEY,
    manufacturer_code VARCHAR(50),
    manufacturer_name VARCHAR(255),
    contact_person VARCHAR(255),
    phone VARCHAR(20),
    email VARCHAR(100),
    address TEXT,
    website VARCHAR(255)
);
```

## Indents

```sql
CREATE TABLE indents (
    indent_id VARCHAR(100) PRIMARY KEY,
    sender_id BIGINT,
    sender_name VARCHAR(255),
    receiver_id BIGINT,
    item_name VARCHAR(255),
    quantity INTEGER,
    priority VARCHAR(20),
    timestamp TIMESTAMP,
    status VARCHAR(20)
);
```

## Notifications

```sql
CREATE TABLE notifications (
    notification_id BIGSERIAL PRIMARY KEY,
    user_id BIGINT,
    title VARCHAR(255),
    message TEXT,
    created_at TIMESTAMP,
    is_read BOOLEAN DEFAULT FALSE
);
```

---

# Realtime Workflow

## Online User Flow

```text
User Login
    ↓
WebSocket Connect
    ↓
/app/user/connect
    ↓
Spring Boot
    ↓
ConcurrentHashMap
    ↓
Broadcast
    ↓
/topic/online-users
    ↓
All Clients Updated
```

---

## Indent Workflow

```text
User 1
   ↓
Create Indent
   ↓
WebSocket Publish
   ↓
/app/sendIndent
   ↓
Spring Boot
   ↓
Save to Database
   ↓
/topic/indent/{receiverId}
   ↓
User 2 Receives Instantly
```

---

## Indent Approval Workflow

```text
Receiver
   ↓
Approve
   ↓
/app/approveIndent
   ↓
Database Update
   ↓
Broadcast Status
   ↓
Sender Receives Update
```

---

# Design Principles Used

- Layered Architecture
- Separation of Concerns
- Dependency Injection
- Repository Pattern
- Service Layer Pattern
- RESTful API Design
- Realtime Event Driven Communication

---

# Installation

## Clone Repository

```bash
git clone <repository-url>
```

---

## Frontend Setup

```bash
cd frontend

npm install

npm run dev
```

Frontend runs on:

```text
http://localhost:5173
```

---

## Backend Setup

```bash
cd backend

./mvnw spring-boot:run
```

Backend runs on:

```text
http://localhost:8081
```

---

## PostgreSQL Configuration

Create database:

```sql
CREATE DATABASE scms;
```

Update:

```properties
application.properties
```

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/scms
spring.datasource.username=postgres
spring.datasource.password=password
```

---

# Future Enhancements

- JWT Authentication
- Role Based Authorization
- Email Notifications
- RabbitMQ Integration
- Redis Caching
- Docker Deployment
- Kubernetes Deployment
- Analytics Dashboard
- Audit Logging

---

# Learning Outcomes

Through this project, the following concepts were implemented and understood:

- React State Management
- TypeScript Interfaces
- REST APIs
- Spring Boot Architecture
- JPA & Hibernate
- PostgreSQL Database Design
- WebSocket Communication
- STOMP Messaging
- Realtime Event Processing
- ConcurrentHashMap Usage
- Layered Architecture
- CRUD Operations
- Authentication Flow

---

# Author

**Ritesh Kumar**

Supply Chain Management System (SCMS)
Built using React, TypeScript, Spring Boot, PostgreSQL and WebSocket Technology.
````
