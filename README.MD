# IKEA Inventory API - Cloud Native Backend

> **Coding Assignment Submission**
> **Role:** Senior Software Engineer - AI | IKEA Range Operations | Enabling Functions
> **Candidate:** Juan Pablo Zúñiga Hidalgo
> **Delivery Date:** Thursday, December 18, 2025

---

## 📝 Overview
This repository contains a  **production-grade RESTful API** designed to manage store inventory with strict data integrity.

While initially developed for local containerized environments, the architecture has been upgraded to a **Cloud-Native approach**, fully compatible with **AWS** infrastructure to ensure high availability, scalability, and security.

---

## ☁️ Cloud Architecture (AWS)
The solution follows the **12-Factor App** methodology to run seamlessly in the cloud:

* **Compute (Stateless):** The API is containerized and deployed on **AWS App Runner** (or ECS), allowing for auto-scaling based on traffic.
* **Persistence (Managed):** Data is stored in **AWS RDS (PostgreSQL 15)**, ensuring automated backups, encryption at rest, and separation of storage from compute.
* **Configuration:** All secrets and DB credentials are injected via **Environment Variables**, keeping the codebase secure.

---

## 🛠️ Technology Stack

* **Runtime:** Node.js 18+ with **TypeScript** (Strict Mode).
* **Framework:** Express.js.
* **Database:** PostgreSQL 15.
* **ORM:** TypeORM (Data Mapper Pattern).
* **Documentation:** Swagger / OpenAPI 3.0.
* **Infrastructure:** Docker & AWS (App Runner + RDS).

---

## 🚀 Getting Started

You can run this project in two ways: **Locally (Docker)** or deploy it to **AWS**.

### Option A: Local Development (Docker)
This spins up the API and a local PostgreSQL container in an isolated network.

1.  **Clone the repository:**
    ```bash
    git clone 
    cd ikea-backend-aws
    ```

2.  **Start the environment:**
    ```bash
    docker-compose up --build
    ```

3.  **Access the application:**
    * **API Status:** `http://localhost:3000`
    * **Swagger Docs:** `http://localhost:3000/api-docs`
    * **Database Port:** `5434` (Mapped externally to avoid conflicts).

---

### Option B: Cloud Deployment (AWS)

To deploy this to **AWS App Runner**:

1.  **Database:** Create a PostgreSQL instance in **AWS RDS**.
2.  **Service:** Create an App Runner service linked to this GitHub repository.
3.  **Build Settings:**
    * Build Command: `npm install && npm run build`
    * Start Command: `npm start`
4.  **Environment Variables:**
    Configure the following secrets in the AWS Console:
    * `DB_HOST`: *(Your RDS Endpoint)*
    * `DB_USERNAME`: `postgres`
    * `DB_PASSWORD`: *(Your Secure Password)*
    * `DB_NAME`: `stock_inventory`
    * `DB_PORT`: `5432`

*The application will automatically synchronize the database schema (TypeORM) upon the first successful deployment.*

---

## 📖 API Documentation

This project includes interactive documentation generated automatically from the code.

* **Local URL:** [http://localhost:3000/api-docs](http://localhost:3000/api-docs)
* **Cloud URL:** `https://<your-aws-app-url>/api-docs`

### Key Endpoints
| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/products` | Create a new product (Unique SKU check). |
| `GET` | `/products` | Retrieve current inventory. |
| `POST` | `/products/{id}/increment` | **Atomic** stock addition. |
| `POST` | `/products/{id}/decrement` | **Safe** stock removal (Prevents negative stock). |

---

## 🧠 Design Decisions & Logic

### 1. Atomic Stock Logic
To prevent race conditions (e.g., during a Black Friday event), stock updates are **not** performed via simple `PUT` requests. Instead, specific "Action" endpoints (`increment`/`decrement`) contain server-side logic to validate:
$$ \text{Current Stock} - \text{Amount} \ge 0 $$
If this condition fails, the transaction is rejected with a `400 Bad Request`.

### 2. Layered Architecture
The code is structured to enforce separation of concerns:
* **Controllers:** Handle HTTP IO and DTO validation.
* **Services:** Contain pure business logic.
* **Repositories:** Isolate direct database access.

### 3. Security
* **Helmet:** Applied for HTTP header security.
* **Input Validation:** `class-validator` ensures incoming data integrity before it reaches the logic layer.
* **No Secrets in Code:** Credentials are managed strictly via `.env` files.

---