Here's a professional README for your **NotificationService_Scms** repository.

````md
# Supply Chain Management System (SCMS)

A full-stack Supply Chain Management System built using React, TypeScript, Spring Boot, PostgreSQL, WebSocket, and RabbitMQ.

## Features

### Authentication
- User Login
- Session Management
- Protected Routes

### Dashboard
- Total Suppliers Count
- Total Materials Count
- Total Manufacturers Count
- Total Employees Count
- Total Stores Count
- Real-time Activity Updates

### Store Management
- Add Store
- Edit Store
- Delete Store
- Store Configuration

### Employee Management
- Add Employee
- Edit Employee
- Delete Employee

### Supplier Management
- Add Supplier
- Edit Supplier
- Delete Supplier

### Material Management
- Add Material
- Edit Material
- Delete Material
- Store Mapping
- Inventory Tracking

### Manufacturer Management
- Add Manufacturer
- Edit Manufacturer
- Delete Manufacturer

### Notifications
- Real-time Notifications
- WebSocket Integration
- RabbitMQ Messaging

### Indent Management
- Create Indents
- Track Requests

---

## Tech Stack

### Frontend
- React
- TypeScript
- Vite
- Tailwind CSS

### Backend
- Spring Boot
- Spring Data JPA
- Hibernate
- PostgreSQL

### Messaging & Realtime
- RabbitMQ
- WebSocket
- STOMP

### Version Control
- Git
- GitHub

---

## Project Structure

```text
NotificationService_Scms
│
├── frontend
│   ├── src
│   │   ├── pages
│   │   ├── components
│   │   ├── services
│   │   ├── layouts
│   │   ├── routes
│   │   └── types
│
├── backend
│   ├── server.js
│   ├── routes
│   └── RabbitMQ Integration
│
├── notification-service
│   ├── controller
│   ├── service
│   ├── repository
│   ├── entity
│   └── websocket
│
└── README.md
```

---

## Backend Setup

### PostgreSQL

Create database:

```sql
CREATE DATABASE notification_db;
```

Configure application properties:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/notification_db
spring.datasource.username=postgres
spring.datasource.password=your_password
```

### Run Spring Boot

```bash
./mvnw spring-boot:run
```

or

```bash
mvn spring-boot:run
```

---

## RabbitMQ Setup

Start RabbitMQ:

```bash
rabbitmq-server
```

Default URL:

```text
amqp://guest:guest@localhost:5672
```

---

## Node Backend Setup

```bash
cd backend

npm install

npm start
```

Server runs on:

```text
http://localhost:5001
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

## API Base URL

```text
http://localhost:8081/api
```

---

## Main Modules

| Module | Status |
|----------|----------|
| Login | ✅ |
| Dashboard | ✅ |
| Store | ✅ |
| Employee | ✅ |
| Supplier | ✅ |
| Material | ✅ |
| Manufacturer | ✅ |
| Notifications | ✅ |
| RabbitMQ | ✅ |
| WebSocket | ✅ |
| Indent | ✅ |

---

## Future Enhancements

- Role Based Access Control (RBAC)
- JWT Authentication
- Audit Logs
- Material Stock Alerts
- Email Notifications
- Reports & Analytics
- Docker Deployment

---

## Author

**Ritesh Kumar**

GitHub:
https://github.com/riteshkumar800

---

## License

This project is developed for learning and demonstration purposes.
````

For the README conflict during rebase, keep this new README and then run:

```bash
git add README.md
git rebase --continue
git push -u origin main
```
