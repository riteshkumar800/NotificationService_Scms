# NotificationService SCMS

A real-time Supply Chain Management System notification project built with Spring Boot, PostgreSQL, React, WebSocket/STOMP, SockJS, and RabbitMQ.

The project manages supply-chain master data such as suppliers, stores, employees, materials, and manufacturers. It also supports real-time user notifications, online-user tracking, and an indent request approval flow.

## Project Summary

This application combines normal REST-based CRUD operations with real-time communication.

- REST APIs are used for master-data management.
- WebSocket/STOMP is used for instant browser updates.
- RabbitMQ is used for asynchronous event-based notifications.
- PostgreSQL is used for persistent storage.
- React TypeScript is used for the frontend dashboard.

## Folder Structure

```text
NotificationService_Scms/
├── backend/              # Node/Express event producer and sample APIs
├── frontend/             # React + TypeScript frontend
└── notificationService/  # Main Spring Boot backend
```

## Main Technologies

| Area | Technology |
| --- | --- |
| Backend | Spring Boot |
| Database | PostgreSQL |
| ORM | Spring Data JPA / Hibernate |
| Real-time communication | WebSocket, STOMP, SockJS |
| Message queue | RabbitMQ |
| Frontend | React, TypeScript, Vite |
| Build tool | Maven |

## Core Features

- User registration and login
- Captcha generation
- Supplier CRUD
- Store CRUD
- Employee CRUD
- Material CRUD
- Manufacturer CRUD
- Real-time user-specific notifications
- Online user tracking
- Real-time indent request flow
- Approve/reject indent updates
- RabbitMQ event consumption
- PostgreSQL persistence

## Architecture

```text
React Frontend
   |
   | REST API / WebSocket
   v
Spring Boot Controllers
   |
   v
Service Layer
   |
   v
Repository Layer
   |
   v
PostgreSQL Database
```

Real-time notification flow:

```text
Frontend sends message to /app/sendMessage
   |
   v
Spring Boot receives it using @MessageMapping
   |
   v
Notification is saved in database
   |
   v
SimpMessagingTemplate sends to /topic/notifications/{receiverId}
   |
   v
Only the target user receives the notification
```

RabbitMQ event flow:

```text
Event producer sends JSON message
   |
   v
RabbitMQ notification_queue
   |
   v
Spring Boot RabbitMQConsumer
   |
   v
WebSocket topic
   |
   v
Frontend receives real-time event
```

## Important Spring Boot Files

| File | Responsibility |
| --- | --- |
| `notificationService/src/main/java/com/demo/notification/NotificationApplication.java` | Starts the Spring Boot application |
| `notificationService/src/main/resources/application.properties` | Database, server, and RabbitMQ configuration |
| `notificationService/src/main/java/com/demo/notification/config/WebSocketConfig.java` | Configures WebSocket endpoint, STOMP broker, and SockJS |
| `notificationService/src/main/java/com/demo/notification/config/RabbitMQConfig.java` | Creates RabbitMQ queue |
| `notificationService/src/main/java/com/demo/notification/service/RabbitMQConsumer.java` | Reads RabbitMQ messages and sends WebSocket notifications |
| `notificationService/src/main/java/com/demo/notification/controller/HomeController.java` | Handles notification, online user, and indent WebSocket flows |
| `notificationService/src/main/java/com/demo/notification/service/OnlineUserService.java` | Tracks online users using `ConcurrentHashMap` |
| `notificationService/src/main/java/com/demo/notification/controller/AuthController.java` | Handles register, login, and captcha APIs |

## REST Modules

The project follows a common layered structure:

```text
Controller -> Service -> Repository -> Entity -> Database
```

Example for suppliers:

| Layer | File |
| --- | --- |
| Controller | `SupplierController.java` |
| Service | `SupplierService.java` |
| Repository | `SupplierRepository.java` |
| Entity | `Supplier.java` |

The same pattern is used for:

- Employees
- Stores
- Materials
- Manufacturers

## API Endpoints

### Authentication

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/auth/register` | Register user |
| `POST` | `/auth/login` | Login user |
| `GET` | `/auth/captcha` | Generate captcha |

### Suppliers

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/api/suppliers` | Get all suppliers |
| `GET` | `/api/suppliers/{id}` | Get supplier by ID |
| `POST` | `/api/suppliers` | Create supplier |
| `PUT` | `/api/suppliers/{id}` | Update supplier |
| `DELETE` | `/api/suppliers/{id}` | Delete supplier |

Similar CRUD endpoints exist for:

- `/api/stores`
- `/api/employees`
- `/api/materials`
- `/api/manufacturers`

### Notifications

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/notifications` | Health/home response |
| `GET` | `/notifications/all` | Get all notifications |
| `POST` | `/notifications` | Create notification |
| `PUT` | `/notifications/{id}` | Update notification |
| `DELETE` | `/notifications/{id}` | Delete notification |

## WebSocket Destinations

WebSocket endpoint:

```text
http://localhost:8081/ws
```

Client sends messages to:

| Destination | Purpose |
| --- | --- |
| `/app/sendMessage` | Send user notification |
| `/app/user/connect` | Register online user |
| `/app/user/disconnect` | Remove online user |
| `/app/sendIndent` | Send indent request |
| `/app/approveIndent` | Approve indent |
| `/app/rejectIndent` | Reject indent |

Client subscribes to:

| Topic | Purpose |
| --- | --- |
| `/topic/notifications/{userId}` | User-specific notification |
| `/topic/online-users` | Online users list |
| `/topic/indent/{userId}` | User-specific indent updates |

## Database Configuration

Database settings are in:

```text
notificationService/src/main/resources/application.properties
```

Example:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/notification_db
spring.datasource.username=riteshkumar
spring.datasource.password=1234
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
server.port=8081
```

For production, credentials should be moved to environment variables.

## RabbitMQ Configuration

RabbitMQ settings:

```properties
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```

Queue name:

```text
notification_queue
```

The queue is configured in `RabbitMQConfig.java` and consumed by `RabbitMQConsumer.java`.

## How To Run

### 1. Start PostgreSQL

Create a database:

```sql
CREATE DATABASE notification_db;
```

Update `application.properties` with your PostgreSQL username and password.

### 2. Start RabbitMQ

RabbitMQ should be running locally on port `5672`.

Default credentials:

```text
username: guest
password: guest
```

### 3. Run Spring Boot Backend

```bash
cd notificationService
./mvnw spring-boot:run
```

Backend runs on:

```text
http://localhost:8081
```

### 4. Run React Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend usually runs on:

```text
http://localhost:5173
```

### 5. Optional: Run Node Backend/Event Producer

```bash
cd backend
npm install
npm start
```

Node backend uses:

```text
http://localhost:5001
```

## Interview Explanation

Short answer:

> This is a real-time Supply Chain Management notification system. It uses Spring Boot and PostgreSQL for backend data management, React TypeScript for the frontend, WebSocket/STOMP for real-time browser updates, and RabbitMQ for asynchronous event messaging. Users can manage suppliers, stores, employees, materials, and manufacturers, send real-time notifications, track online users, and raise or approve indents.

Detailed answer:

> The project follows layered architecture. Controllers handle incoming REST and WebSocket requests. Services contain business logic such as setting default status, timestamps, saving notifications, and updating indent status. Repositories communicate with PostgreSQL through Spring Data JPA. For real-time notifications, the frontend connects to `/ws`, sends messages to `/app/...`, and subscribes to `/topic/...`. The backend uses `SimpMessagingTemplate` to send messages to specific users. RabbitMQ is used for asynchronous backend event handling, where messages are consumed from `notification_queue` and forwarded to the frontend through WebSocket.

## Important Interview Questions

### What is the purpose of this project?

It manages supply-chain master data and provides real-time notifications for user actions, online users, and indent approvals.

### Why did you use WebSocket?

WebSocket allows the server to push messages instantly to the browser without page refresh or repeated polling.

### What is STOMP?

STOMP is a messaging protocol used over WebSocket. In this project, clients send messages to `/app/...` and subscribe to `/topic/...`.

### What is SockJS?

SockJS provides fallback support if native WebSocket is unavailable or blocked.

### Why did you use RabbitMQ?

RabbitMQ is used for asynchronous event-based communication. It helps decouple event producers from the notification service.

### Difference between RabbitMQ and WebSocket?

RabbitMQ is mainly server-to-server asynchronous messaging. WebSocket is server-to-browser real-time communication.

### Why use a service layer?

The service layer keeps business logic separate from controller logic. This improves maintainability and testing.

### Why use `ConcurrentHashMap` for online users?

Multiple users can connect and disconnect at the same time. `ConcurrentHashMap` is thread-safe, so it avoids concurrency issues.

### What happens if a user is offline?

Current real-time WebSocket delivery works only when the user is connected. For better offline support, notifications should be stored with read/unread status and fetched when the user logs in.

### Is authentication production-ready?

No. Current authentication is basic. Production should use Spring Security, BCrypt password hashing, JWT tokens, role-based authorization, and secure token expiry.

### Is `ddl-auto=update` good for production?

No. It is useful during development. Production should use controlled migration tools like Flyway or Liquibase.

### How would you improve this project?

- Add Spring Security and JWT
- Hash passwords with BCrypt
- Add DTOs for all request/response models
- Add validation using `@Valid`
- Add global exception handling with `@ControllerAdvice`
- Restrict CORS to trusted frontend URLs
- Add unread notification support
- Add RabbitMQ retry and dead-letter queue handling
- Remove old commented code
- Add unit and integration tests
- Use environment variables for credentials

## Production Considerations

Before using this project in production, improve:

- Security
- Password handling
- CORS rules
- Error handling
- Input validation
- Database migration process
- WebSocket authorization
- Queue retry/dead-letter behavior
- Logging and monitoring
- Test coverage

