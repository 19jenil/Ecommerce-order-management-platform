# E-Commerce Order Management Platform

A full-stack e-commerce and order management platform built with **Java, Spring Boot, React, TypeScript, PostgreSQL, MongoDB, RabbitMQ, Docker, and Kubernetes**.

The platform supports secure authentication, product and category management, shopping carts, checkout, order history, asynchronous order-event processing, and audit logging. It combines relational and document-based persistence and provides containerized development and deployment workflows.

> **Portfolio project:** This repository represents a customized and extended version of an existing e-commerce codebase, with additional work focused on backend configuration, persistence, event-driven processing, containerization, testing, and deployment workflows.

---

## 🚀 Key Features

- 🔐 **JWT Authentication & Authorization**
  - User registration and login
  - JWT-based authentication
  - Role-based access control for `USER` and `ADMIN`

- 🛍️ **Product & Category Management**
  - Product CRUD operations
  - Product image upload
  - Category management
  - Product search

- 🛒 **Shopping Cart & Checkout**
  - Persistent server-side shopping cart
  - Add, update, and remove cart items
  - Transactional checkout workflow
  - Automatic cart clearing after successful checkout

- 📦 **Order Management**
  - Order creation
  - Order line items
  - Order status tracking
  - Per-user order history
  - Detailed order retrieval

- ⚡ **Event-Driven Processing**
  - RabbitMQ-based asynchronous order events
  - `OrderCreatedEvent` messaging
  - Decoupled audit processing using RabbitMQ consumers

- 📊 **Audit Logging**
  - MongoDB-based audit/activity storage
  - Order event auditing
  - Product-search auditing

- 🗄️ **Polyglot Persistence**
  - PostgreSQL for transactional application data
  - MongoDB for audit/activity data
  - Flyway database migrations

- 🐳 **Containerization**
  - Dockerized backend and frontend
  - Docker Compose for local full-stack environments
  - Health checks and persistent database volumes

- ☸️ **Kubernetes Deployment**
  - Kubernetes Deployments and Services
  - ConfigMaps and Secrets
  - PostgreSQL and MongoDB persistent volumes
  - RabbitMQ deployment

- 🧪 **Automated Testing**
  - JUnit 5
  - Mockito
  - Testcontainers
  - PostgreSQL/MongoDB integration testing

- 🔄 **CI/CD**
  - GitHub Actions
  - GitLab CI
  - Backend build and test automation
  - Frontend linting and build validation

---

# 🏗️ Architecture

```text
                         ┌─────────────────────────────┐
                         │     React + TypeScript       │
                         │       Vite Frontend         │
                         │   Tailwind / UI Components  │
                         └──────────────┬──────────────┘
                                        │
                                   REST + JWT
                                        │
                                        ▼
                    ┌─────────────────────────────────────┐
                    │        Spring Boot Backend          │
                    │                                     │
                    │  Auth │ Products │ Categories       │
                    │  Cart │ Orders   │ Audit            │
                    └───────────────┬───────────┬─────────┘
                                    │           │
                              Sync  │           │ Async
                                    │           │
                                    ▼           ▼
                         ┌───────────────┐   ┌───────────────┐
                         │  PostgreSQL   │   │   RabbitMQ    │
                         │               │   │               │
                         │ Users         │   │ Order Events  │
                         │ Products      │   │               │
                         │ Cart          │   └───────┬───────┘
                         │ Orders        │           │
                         └───────────────┘           ▼
                                            ┌────────────────┐
                                            │ Audit Consumer │
                                            └───────┬────────┘
                                                    │
                                                    ▼
                                             ┌─────────────┐
                                             │   MongoDB   │
                                             │ Audit Logs  │
                                             └─────────────┘
```

### Order Event Flow

The checkout workflow follows a synchronous transaction followed by asynchronous event processing:

```text
User Checkout
     │
     ▼
POST /api/orders
     │
     ├── Create Order
     ├── Create Order Items
     ├── Clear Cart
     ├── Write Audit Entry
     │
     ▼
Publish OrderCreatedEvent
     │
     ▼
RabbitMQ
     │
     ▼
@RabbitListener
     │
     ▼
MongoDB Audit Log
```

This separates the core checkout transaction from downstream audit processing.

---

# 🧰 Technology Stack

## Backend

| Technology | Purpose |
|---|---|
| Java 21 | Application development |
| Spring Boot | Backend framework |
| Spring Security | Authentication & authorization |
| JWT | Stateless authentication |
| Spring Data JPA | Relational persistence |
| PostgreSQL | Transactional database |
| Flyway | Database migrations |
| Spring Data MongoDB | Audit persistence |
| MongoDB | Audit/activity database |
| Spring AMQP | RabbitMQ integration |
| RabbitMQ | Asynchronous messaging |
| Maven | Build & dependency management |
| Lombok | Boilerplate reduction |
| Springdoc OpenAPI | API documentation |

## Frontend

| Technology | Purpose |
|---|---|
| React | UI development |
| TypeScript | Type-safe frontend development |
| Vite | Frontend tooling |
| React Router | Client-side routing |
| Axios | HTTP/API communication |
| Tailwind CSS | Styling |
| shadcn/ui-style components | Reusable UI components |
| Context API | Application state |

## DevOps & Infrastructure

| Technology | Purpose |
|---|---|
| Docker | Application containerization |
| Docker Compose | Local multi-container environment |
| Kubernetes | Container orchestration |
| GitHub Actions | CI/CD |
| GitLab CI | CI/CD |
| Kubernetes ConfigMaps | Configuration |
| Kubernetes Secrets | Sensitive configuration |
| Persistent Volumes | Database persistence |

## Testing

| Technology | Purpose |
|---|---|
| JUnit 5 | Unit testing |
| Mockito | Mock-based testing |
| Testcontainers | Integration testing |
| Maven Surefire | Test execution |

---

# 📁 Project Structure

```text
Ecommerce-order-management-platform/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── Ecommerce-Backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/cart/ecom_proj/
│   │   │   │
│   │   │   ├── audit/
│   │   │   ├── cart/
│   │   │   ├── category/
│   │   │   ├── config/
│   │   │   ├── controller/
│   │   │   ├── dto/
│   │   │   ├── model/
│   │   │   ├── order/
│   │   │   ├── repo/
│   │   │   ├── security/
│   │   │   └── service/
│   │   │
│   │   └── resources/
│   │       ├── db/migration/
│   │       ├── application.properties
│   │       ├── application-dev.properties
│   │       └── application-docker.properties
│   │
│   ├── Dockerfile
│   └── pom.xml
│
├── Ecommerce-Frontend/
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── types/
│   │   ├── Context/
│   │   ├── App.tsx
│   │   └── main.tsx
│   │
│   ├── Dockerfile
│   └── package.json
│
├── k8s/
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   ├── postgres-deployment.yaml
│   ├── postgres-pvc.yaml
│   ├── mongodb-deployment.yaml
│   ├── mongodb-pvc.yaml
│   ├── rabbitmq-deployment.yaml
│   ├── configmap.yaml
│   └── secret.yaml
│
├── docker-compose.yml
├── .gitignore
├── .gitlab-ci.yml
└── README.md
```

---

# 🔑 Authentication

The backend uses **Spring Security and JWT** for stateless authentication.

### Authentication Flow

```text
Register
   │
   ▼
POST /api/auth/register
   │
   ▼
User stored in PostgreSQL
   │
   ▼
Login
   │
   ▼
POST /api/auth/login
   │
   ▼
JWT Token
   │
   ▼
Frontend stores authentication state
   │
   ▼
JWT attached to protected API requests
```

Administrative endpoints require the appropriate `ADMIN` role.

---

# 🔌 REST API

## Authentication

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register a user |
| POST | `/api/auth/login` | Authenticate and receive JWT |

## Products

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/products` | Retrieve products |
| GET | `/api/products/{id}` | Retrieve product |
| GET | `/api/products/search?keyword=` | Search products |
| POST | `/api/products` | Create product |
| PUT | `/api/products/{id}` | Update product |
| DELETE | `/api/products/{id}` | Delete product |

Product administration endpoints require `ADMIN` authorization.

## Categories

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/categories` | List categories |
| GET | `/api/categories/{id}` | Get category |
| POST | `/api/categories` | Create category |
| PUT | `/api/categories/{id}` | Update category |
| DELETE | `/api/categories/{id}` | Delete category |

## Cart

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/cart` | Get authenticated user's cart |
| POST | `/api/cart` | Add/increment cart item |
| DELETE | `/api/cart/{cartItemId}` | Remove cart item |

## Orders

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/orders` | Checkout and create order |
| GET | `/api/orders/{userId}` | Retrieve order history |
| GET | `/api/orders/detail/{orderId}` | Retrieve order details |

---

# 🗄️ Data Architecture

The application uses two persistence technologies for different workloads.

### PostgreSQL

Used for transactional application data:

```text
Users
Products
Categories
Cart
Cart Items
Orders
Order Items
```

Flyway manages database schema migrations.

### MongoDB

Used for audit/activity data:

```text
AuditLog
   ├── Order events
   └── Product search activity
```

This separation demonstrates **polyglot persistence**, using each database according to the workload.

---

# 📨 Event-Driven Processing

RabbitMQ is used to decouple order events from downstream audit processing.

When checkout succeeds:

```text
Order Service
     │
     ▼
OrderCreatedEvent
     │
     ▼
RabbitMQ Exchange
     │
     ▼
order.created.queue
     │
     ▼
OrderEventListener
     │
     ▼
MongoDB AuditLog
```

This allows audit processing to occur asynchronously instead of making it part of the primary checkout response path.

---

# 🐳 Running with Docker Compose

### Prerequisites

- Docker Desktop
- Docker Compose
- Git

Clone the repository:

```bash
git clone https://github.com/19jenil/Ecommerce-order-management-platform.git
cd Ecommerce-order-management-platform
```

Start the full stack:

```bash
docker compose up --build
```

The environment includes:

```text
Frontend       → http://localhost:5173
Backend        → http://localhost:8080
Swagger UI     → http://localhost:8080/swagger-ui.html
RabbitMQ UI    → http://localhost:15672
PostgreSQL     → localhost:5432
MongoDB        → localhost:27017
```

> Development credentials and secrets in configuration files are intended for local development only. Production deployments should provide secrets through environment variables or a secure secret-management solution.

Stop the environment:

```bash
docker compose down
```

Remove containers and persistent volumes:

```bash
docker compose down -v
```

---

# 💻 Local Backend Development

Navigate to the backend:

```bash
cd Ecommerce-Backend
```

Run:

```bash
mvn spring-boot:run
```

The default development profile uses an in-memory H2 database.

Swagger UI:

```text
http://localhost:8080/swagger-ui.html
```

H2 Console:

```text
http://localhost:8080/h2-console
```

---

# 🎨 Local Frontend Development

Navigate to the frontend:

```bash
cd Ecommerce-Frontend
```

Install dependencies:

```bash
npm install
```

Run the development server:

```bash
npm run dev
```

Build the application:

```bash
npm run build
```

Run linting:

```bash
npm run lint
```

---

# 🧪 Testing

## Backend Unit Tests

```bash
cd Ecommerce-Backend
mvn test
```

Unit tests use JUnit 5 and Mockito.

## Integration Tests

The project includes Testcontainers-based integration testing for database-backed workflows.

```bash
mvn test -Dtest='*IT'
```

The integration environment can provision services such as PostgreSQL and MongoDB through Docker.

## Frontend Validation

```bash
cd Ecommerce-Frontend

npm run lint
npm run build
```

---

# ☸️ Kubernetes Deployment

Kubernetes manifests are provided under:

```text
k8s/
```

The deployment includes:

```text
Namespace
├── Backend Deployment + Service
├── Frontend Deployment + Service
├── PostgreSQL Deployment + PVC + Service
├── MongoDB Deployment + PVC + Service
├── RabbitMQ Deployment + Service
├── ConfigMap
└── Secret
```

Apply the manifests:

```bash
kubectl apply -f k8s/
```

Check workloads:

```bash
kubectl get pods
kubectl get services
```

> The included Kubernetes secret contains example values only. Production deployments should use a secure secret-management mechanism.

---

# 🔄 CI/CD

The repository includes automated CI configuration.

### GitHub Actions

The workflow performs:

```text
Push / Pull Request
        │
        ├── Backend unit tests
        ├── Integration tests
        ├── Maven package
        │
        └── Frontend
             ├── npm ci
             ├── lint
             └── build
```

Workflow:

```text
.github/workflows/ci.yml
```

### GitLab CI

A corresponding GitLab pipeline is provided through:

```text
.gitlab-ci.yml
```

---

# 🔧 Engineering Work & Customization

This repository was used as a base for hands-on engineering work and customization.

Key areas of work include:

- Refactoring persistence configuration for PostgreSQL compatibility
- Correcting binary image persistence using PostgreSQL `bytea`
- Improving environment-based JWT configuration
- Removing hardcoded JWT secrets from application configuration
- Working with Docker Compose across backend, frontend, PostgreSQL, MongoDB, and RabbitMQ
- Configuring asynchronous order-event processing
- Working with Kubernetes deployment manifests
- Validating backend and integration testing workflows
- Preparing the project for a clean, reproducible development environment

The repository is intended to demonstrate practical experience across **backend development, full-stack integration, databases, messaging, containerization, testing, and deployment**.

---

# 📈 Future Improvements

Potential next steps include:

- Extracting backend domains into independently deployable microservices
- Introducing an API gateway
- Adding centralized configuration management
- Adding distributed tracing and observability
- Introducing Redis caching
- Adding automated container image publishing
- Adding cloud deployment
- Improving frontend test coverage
- Adding end-to-end browser testing
- Implementing stronger production secret management
- Adding structured application logging and metrics

---

# 📚 Learning Outcomes

This project provides hands-on exposure to:

- REST API development
- Spring Boot application architecture
- Secure JWT authentication
- Relational database design
- NoSQL data modeling
- Database migrations
- Event-driven architecture
- Message brokers
- Transaction management
- Unit and integration testing
- Testcontainers
- Docker
- Kubernetes
- CI/CD
- React and TypeScript
- Full-stack application integration

---

## 👨‍💻 Author

**Jenil Patel**


GitHub: https://github.com/19jenil

---

## 📄 License

This repository is intended primarily as a learning and portfolio project.