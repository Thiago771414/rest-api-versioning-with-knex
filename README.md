# Express REST API with Knex, SQLite, and API Versioning

![Project Type](https://img.shields.io/badge/Project%20Type-Backend%20Learning%20Case%20Study-purple)
![Backend](https://img.shields.io/badge/Backend-Node.js-green)
![Framework](https://img.shields.io/badge/Framework-Express-black)
![Database](https://img.shields.io/badge/Database-SQLite-blue)
![Query Builder](https://img.shields.io/badge/Query%20Builder-Knex-orange)
![API](https://img.shields.io/badge/API-REST-red)
![Versioning](https://img.shields.io/badge/Versioning-v1%20%26%20v2-purple)
![Status](https://img.shields.io/badge/Status-Learning%20Project-blue)

A backend learning project that demonstrates how to build a **REST API with Express.js**, evolve it from **in-memory data** to **persistent SQLite storage**, and organize database access using **Knex**, **migrations**, and **seeds**.

This repository is designed as a practical example of how a simple CRUD API can gradually become more structured and closer to a real backend application.

---

<p align="center">
  <img src="https://raw.githubusercontent.com/Thiago771414/imagensProjetos/main/slices/mobile/restAPIversioningKnex.png" width="900">
</p>

---

# Case Study

## Problem

When learning backend development with Node.js, many projects start with hardcoded arrays and simple route handlers.

While this is useful for understanding the basics, it does not reflect how real applications handle:

- persistent data storage
- schema evolution
- repeatable database setup
- API versioning
- maintainable database access

As the application grows, using only in-memory data quickly becomes limited and difficult to scale.

---

## Solution

This project demonstrates a step-by-step evolution of an Express API:

- **Version 1** uses in-memory product data for fast CRUD development
- **Version 2** replaces hardcoded data with **SQLite + Knex**
- **Knex migrations** define the database schema
- **Knex seeds** populate initial data
- the API remains versioned to illustrate progressive backend design

This approach helps developers understand both the simplicity of early API development and the transition to a more realistic persistence layer.

---

## Business / Learning Value

This project is useful for developers who want to understand:

- how REST APIs are structured in Express
- how CRUD operations work in practice
- how to use Knex with SQLite
- how migrations and seeds improve repeatability
- how API versioning can support incremental evolution
- how to replace mock data with real database access

It is especially valuable as a learning bridge between **basic Express routing** and **real-world backend persistence patterns**.

---

# Architecture

The project follows a simple backend architecture with versioned routes and a SQLite persistence layer.

```text
Client / HTTP Tool
        │
        ▼
Express Application
        │
        ├── /api/v1  → In-memory product API
        │
        └── /api/v2  → Knex-powered product API
                         │
                         ▼
                    SQLite Database
                         │
               ┌─────────┴─────────┐
               ▼                   ▼
         Migrations            Seed Data
```

<p align="center">
  <img src="https://raw.githubusercontent.com/Thiago771414/imagensProjetos/main/slices/mobile/mermaid-diagram.png" width="900">
</p>

---

```text
project-root/
│
├── app.js
├── knexfile.js
├── package.json
│
├── routes/
│   ├── index.js
│   ├── users.js
│   ├── apiRouterV1.js
│   └── apiRouterV2.js
│
├── db/
│   ├── dev.sqlite3
│   ├── migrations/
│   └── seeds/
│
├── public/
├── views/
└── README.md
```

---

# Key Features

Express.js REST API

API versioning (v1 and v2)

CRUD operations for products

SQLite persistence

Knex query builder

database migrations

seed-based initial data loading

transition from mock data to persistent storage

---

# Tech Stack

Backend

Node.js

Express.js

Database

SQLite

Database Access

Knex

Utilities

Morgan

Cookie Parser

HTTP Errors

---

# API Versions

Version 1 — In-Memory API

This version stores products in a local JavaScript array.

Endpoints:
```text
GET /api/v1/produtos

GET /api/v1/produtos/:id

POST /api/v1/produtos

PUT /api/v1/produtos/:id

DELETE /api/v1/produtos/:id
```
Version 2 — Knex + SQLite API

This version uses SQLite and Knex for data persistence.

Endpoints:

```text
GET /api/v2/produtos

GET /api/v2/produtos/:id

POST /api/v2/produtos

PUT /api/v2/produtos/:id

DELETE /api/v2/produtos/:id
```
---

# How to Run

1. Install dependencies:
   
```bash
npm install
```

If needed:

```bash
npm install cookie-parser
npm install knex sqlite3
```

2. Initialize Knex:

```bash
npx knex init
```

3. Configure the database

Update knexfile.js to use SQLite inside the db folder:

```Js
development: {
  client: 'sqlite3',
  connection: {
    filename: './db/dev.sqlite3'
  },
  useNullAsDefault: true,
  migrations: {
    directory: './db/migrations'
  },
  seeds: {
    directory: './db/seeds'
  }
}
```

4. Run migrations

```bash
npx knex migrate:latest
```

5. Run seeds

```bash
npx knex seed:run
```

6. Start the server

```bash
npm start
```
or
```bash
node app.js
```

The application should be available at:

```bash
http://localhost:3000
```

# Database Setup
Migration example

```js
exports.up = function(knex) {
  return knex.schema.createTable("produtos", tbl => {
    tbl.increments('id');
    tbl.text("descricao", 255).unique().notNullable();
    tbl.text("marca", 128).notNullable();
    tbl.decimal("preco").notNullable();
  });
};

exports.down = function(knex) {
  return knex.schema.dropTableIfExists('produtos');
};
```
# Seed example

```js
exports.seed = async function(knex) {
  await knex('produtos').del();

  await knex('produtos').insert([
    { "descricao": "camiseta", "marca": "Nike", "preco": 49.99 },
    { "descricao": "calça jeans", "marca": "Levi's", "preco": 89.95 },
    { "descricao": "tênis esportivo", "marca": "Adidas", "preco": 79.50 },
    { "descricao": "vestido floral", "marca": "Zara", "preco": 59.99 },
    { "descricao": "moletom com capuz", "marca": "Puma", "preco": 69.75 },
    { "descricao": "boné", "marca": "New Era", "preco": 29.99 },
    { "descricao": "bolsa de couro", "marca": "Michael Kors", "preco": 149.00 },
    { "descricao": "óculos de sol", "marca": "Ray-Ban", "preco": 119.50 },
    { "descricao": "shorts jeans", "marca": "Guess", "preco": 54.95 },
    { "descricao": "jaqueta de couro", "marca": "Harley Davidson", "preco": 199.99 }
  ]);
};
```
# Example Request Flow

Version 1

```text
Request → Express Route → In-Memory Array → JSON Response
```

Version 2

```text
Request → Express Route → Knex Query → SQLite → JSON Response
```
---

# What This Project Demonstrates

This repository demonstrates practical knowledge of:

Express routing

RESTful API structure

CRUD design

API versioning

SQLite integration

Knex query building

migrations and seeds

backend evolution from mock storage to persistent storage

---

Suggested Improvements

For a more production-oriented version, the next steps could include:

request validation with Joi or Zod

service layer and repository layer

centralized error handling

environment variables with dotenv

automated tests

authentication and authorization

Docker support

Swagger / OpenAPI documentation

---

# Author

Thiago Reis Lima
Software Engineer

LinkedIn:
```bash
https://www.linkedin.com/in/thiago-lima-2a5896166/
```
