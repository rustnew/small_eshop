Small eShop Backend
Overview
Small eShop Backend is a robust RESTful API for an e-commerce platform, built with Rust to ensure performance, safety, and reliability. It provides core functionalities for managing users, categories, and products, making it ideal for small to medium-sized online stores. The backend leverages Actix-Web for high-performance web handling, SQLx for type-safe database interactions, and PostgreSQL for persistent storage.
Features

User Management: Create and retrieve user accounts with validation for email, first name, last name, and role.
Category Management: CRUD operations for product categories, including name and description.
Product Management: CRUD operations for products with support for category association, precise monetary calculations using BigDecimal, and inventory tracking.
Database Integrity: Enforces foreign key constraints (e.g., category_id in products) with cascading deletes and check constraints for positive prices and non-negative stock.
Error Handling: Comprehensive error handling with detailed logging and HTTP status codes (e.g., 400 for invalid input, 404 for not found).
Scalability: Designed with performance in mind, using async/await for non-blocking operations.

Tech Stack

Language: Rust (Edition 2021)
Web Framework: Actix-Web 4.9
Database: PostgreSQL with SQLx 0.7 (async, type-safe queries)
Dependencies:
uuid for unique identifiers
chrono for timestamp management
sqlx::types::BigDecimal for precise monetary calculations
serde for JSON serialization/deserialization
log and env_logger for logging
dotenv for environment configuration



Database Schema
The backend uses two main tables:

categories:

id: UUID (primary key)
name: Text (not null)
description: Text (not null)
created_at, updated_at: Timestamps (not null)


products:

id: UUID (primary key)
category_id: UUID (foreign key referencing categories, cascades on delete)
name, description, image_url: Text (not null)
price_av, price_ap: Decimal (not null, positive)
stock_quantity: Integer (not null, non-negative)
created_at, updated_at: Timestamps (not null)



API Endpoints
Categories

POST /api/categories: Create a category
GET /api/categories: List all categories
GET /api/categories/{id}: Get a category by ID
PATCH /api/categories/{id}: Update a category
DELETE /api/categories/{id}: Delete a category (cascades to products)

Products

POST /api/products: Create a product
GET /api/products: List all products
GET /api/products/{id}: Get a product by ID
PATCH /api/products/{id}: Update a product
DELETE /api/products/{id}: Delete a product

Users

POST /api/users: Create a user
GET /api/users: List all users

Setup

Prerequisites:

Rust (stable, edition 2021)
PostgreSQL
Cargo


Clone the Repository:
git clone https://github.com/your-username/small-eshop-backend.git
cd small-eshop-backend


Configure Environment:Create a .env file:
DATABASE_URL=postgres://username:password@localhost:5432/small_eshop


Set Up Database:Apply migrations (if using SQLx migrations):
cargo sqlx database create
cargo sqlx migrate run


Run the Application:
cargo run

The API will be available at http://127.0.0.1:8080.


Testing
Test endpoints using curl or tools like Postman. Example:
curl -X POST http://127.0.0.1:8080/api/products \
-H "Content-Type: application/json" \
-d '{"category_id":"550e8400-e29b-41d4-a716-446655440000","name":"Smartphone","description":"A high-end smartphone","price_av":"699.99","price_ap":"799.99","stock_quantity":50,"image_url":"https://example.com/smartphone.jpg"}'

Future Improvements

Add authentication (e.g., JWT) for secure endpoints.
Implement pagination and filtering for product listings.
Add API documentation with OpenAPI/Swagger.
Introduce SQL triggers for automatic updated_at timestamps.

