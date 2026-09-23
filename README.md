# E-Commerce Microservices Configuration Repository

Centralized external configuration repository for Spring Cloud Config Server managing the configuration profiles of all microservices in the e-commerce platform.

---

## 🏗️ Architecture & Services Overview

| Service | Config File | Port | Dependencies / Storage | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Discovery Server** | `discovery-server.yml` | `8761` | Eureka Server | Service registration and discovery |
| **API Gateway** | `gateway-service.yml` | `8222` | Spring Cloud Gateway | Central entry point and request routing |
| **Customer Service** | `customer-service.yml` | `8090` | MongoDB (`customer`) | Customer profile management |
| **Product Service** | `products-service.yml` | `8050` | PostgreSQL (`product`), Flyway | Product catalog and inventory management |
| **Order Service** | `order-service.yml` | `8070` | PostgreSQL (`order`), Kafka | Order processing & orchestrator |
| **Payment Service** | `payment-service.yml` | `8060` | PostgreSQL (`payment`), Kafka | Payment processing |
| **Notification Service** | `notification-service.yml` | `8040` | MongoDB (`notification`), Kafka, Mail | Email notifications for orders & payments |
| **Global / Shared** | `application.yml` | - | Eureka Client | Shared defaults across all services |

---

## ⚙️ Environment Variables & Credentials

The configuration files use environment variables with sensible local defaults:

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `MONGO_USER` | `nabgha` | MongoDB username |
| `MONGO_PASSWORD` | `password` | MongoDB password |
| `POSTGRES_USER` | `nabgha` | PostgreSQL username |
| `POSTGRES_PASSWORD` | `password` | PostgreSQL password |
| `MAIL_USER` | `nabgha` | SMTP mail username |
| `MAIL_PASSWORD` | `password` | SMTP mail password |

---

## 🚀 How It Works

1. **Spring Cloud Config Server** is configured to point to this Git repository:
   ```yaml
   spring:
     cloud:
       config:
         server:
           git:
             uri: <path-or-url-to-this-repo>
             default-label: main
   ```
2. When a microservice boots up (e.g., `customer-service`), it fetches its specific configuration (`customer-service.yml`) along with the global configuration (`application.yml`) from the Config Server.
3. Centralized changes pushed to this repository can be dynamically refreshed across the microservices ecosystem.
