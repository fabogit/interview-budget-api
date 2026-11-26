# interview-budget-api

> A Node.js and Express REST microservice providing personal budget tracking, financial transaction persistence via the official MongoDB driver, and Postman API contract testing.

**Inception (First Commit):** `2022-05-23` • **Batch / Horizon:** `batch-2021-2022` • **Architecture Domain:** `Expense Tracking / MongoDB REST Services`

---

## 📌 Architecture & Overview

`JobInterview_api-budget` provides a lightweight expense and income management REST API built as an interview assessment service. Built on Express 4 and the official MongoDB native driver (`mongodb` 4.6), it demonstrates active database connection pooling, Active Record-style domain modeling (`Transaction`), and endpoint routing.

The service decouples server bootstrapping (`start.js`), application middleware registration (`server.js`), relational routing (`routes/transaction.route.js`), and database lifecycle management (`data/database.js`).

```
                              +-----------------------------------------------+
                              |           JobInterview_api-budget             |
                              |                                               |
[ HTTP Client / Postman ]     |  +-------------------+                        |
        |                     |  | Morgan Logger     |                        |
        | HTTP (GET / POST)   |  +---------+---------+                        |
        v                     |            |                                  |
+---------------------------+ |  +---------v---------+                        |
| Express JSON & CORS       +-+->+ TransactionRouter |                        |
+---------------------------+ |  +---------+---------+                        |
                              |            |                                  |
                              |            v                                  |
                              |  +-------------------+                        |
                              |  | TransactionCtrl   |                        |
                              |  +---------+---------+                        |
                              |            |                                  |
                              |            v                                  |
                              |  +-------------------+                        |
                              |  | Transaction Model |                        |
                              |  +---------+---------+                        |
                              |            |                                  |
                              |            v                                  |
                              |  +-------------------+                        |
                              |  | MongoDB Database  |                        |
                              |  | (MongoClient)     |                        |
                              |  +-------------------+                        |
                              +-----------------------------------------------+
```

* **Context:** Technical interview project designed to evaluate Node.js fundamentals, clean architectural layering, native database client handling without ORM/ODM abstractions, and automated HTTP testing with Supertest.
* **Key Patterns:**
  * **Decoupled Server Bootstrapping:** Separation of `server.js` (Express app configuration) from `start.js` (network port binding and asynchronous database connection initialization).
  * **Active Record Domain Pattern:** The `Transaction` model encapsulates both data properties and persistence operations (`findAll`, `addTransaction`).
  * **Native MongoDB Driver Integration:** Direct connection management and document mutation using `mongodb.MongoClient`.

---

## 🛠 Tech Stack

* **Runtime & Framework:** Node.js (>= 16), Express 4.18.1
* **Database & Persistence:** MongoDB Native Driver (`mongodb` 4.6.0)
* **Configuration & Middleware:** `dotenv` 16.0.1, `cors` 2.8.5, `morgan` 1.10.0
* **Testing & Tooling:** Jest 28.1.0, Supertest 6.2.3, Nodemon 2.0.16, ESLint 8.16.0

---

## ✨ Features & Endpoints

* **Health Verification:** Ping route returning connection status confirmation.
* **Transaction Retrieval:** Historical document querying from the MongoDB transactions collection.
* **Transaction Creation:** Persistence of classified financial entries (income vs. expense, amount, categories, origin, destination).
* **API Testing Suite:** Preconfigured Postman collection and environment definitions.

### Key API Routes / Interfaces

| Method | Route | Description |
|---|---|---|
| `GET` | `/transactions/test` | Connectivity verification probe returning status code 200. |
| `GET` | `/transactions` | Retrieve all recorded budget transactions from MongoDB. |
| `POST` | `/transactions/create` | Insert a new financial transaction document. |

---

## 🚀 Getting Started

### Prerequisites

* Node.js `>= 16.0.0`
* Active MongoDB server instance (Local or MongoDB Atlas)

### Installation

```bash
# Clone the repository
git clone https://github.com/fabogit/interview-budget-api.git
cd interview-budget-api

# Install dependencies
npm install
```

### Environment Configuration

Rename the template file `renameTo.env` to `.env` and configure your MongoDB connection string:

```env
PORT=3000
nodeServer_host=http://localhost:3000
nodeServer_port=3000
NODE_ENV=development

mongoDb_uriCloud=mongodb://localhost:27017
mongoDb_databaseName=budget_db
mongoDb_collectionTransactions=transactions
```

### Running the Application

```bash
# Development mode with hot-reload via nodemon
npm run dev

# Production startup
npm start
```

### Running Tests

```bash
# Run unit & integration tests with Jest
npm test

# Watch mode
npm run test:watch
```

---

## 📄 License & Author

Developed by [@fabogit](https://github.com/fabogit).
