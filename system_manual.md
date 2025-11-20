# 📘 SYSTEM DOCUMENTATION  
## MARKETPLACE SYSTEM

## 1. Project Information

| Field | Value |
|-------|--------|
| **Project Name** | Marketplace System |
| **Student Name** | Loreley González Iguarán |
| **Course** | Ingeniería de Sistemas |
| **Semester** | 7 |
| **Date** | 2025 |
| **Instructor** | Jaider Quintero |

### Short Project Description  
The database is designed for an e-commerce marketplace system that manages products, sellers, customers, orders, payments, shipments, and reviews. The design follows a normalized relational model with clear relationships between entities.

### Entity Relationship Diagram
┌─────────────┐       ┌─────────────┐       ┌─────────────┐
│   Seller    │       │   Product   │       │  Category   │
├─────────────┤       ├─────────────┤       ├─────────────┤
│ id (PK)     │┼────┼│ id (PK)     │   ┼──┼│ id (PK)     │
│ name        │       │ seller_id   │       │ name        │
│ email       │       │ category_id │       │ description │
│ password    │       │ name        │       │ created_at  │
│ created_at  │       │ description │       └─────────────┘
└─────────────┘       │ price       │              │
         │            │ stock       │              │
         │            │ created_at  │              │
         │            │ updated_at  │              │
         │            └─────────────┘              │
         │                   │                     │
         │                   │                     │
         │            ┌──────┴──────┐              │
         │            │             │              │
         │      ┌─────┴─────┐ ┌─────┴─────┐        │
         │      │   Tag     │ │  Review   │        │
         │      ├───────────┤ ├───────────┤        │
         │      │ id (PK)   │ │ id (PK)   │        │
         └──────┤ product_id│ │ product_id│        │
                │ tag_id    │ │ customer_i│        │
                │ created_at│ │ rating    │        │
                └───────────┘ │ comment   │        │
                         │ created_at │        │
                         └───────────┘        │
                                  │            │
                                  │            │
                         ┌────────┴────────┐   │
                         │ Product_Tag     │   │
                         ├─────────────────┤   │
                         │ id (PK)         │   │
                         │ product_id (FK) │   │
                         │ tag_id (FK)     │   │
                         │ created_at      │   │
                         └─────────────────┘   │
                                               │
┌─────────────┐       ┌─────────────┐         │
│  Customer   │       │   Order     │         │
├─────────────┤       ├─────────────┤         │
│ id (PK)     │┼────┼│ id (PK)     │         │
│ name        │       │ customer_id │         │
│ email       │       │ order_date  │         │
│ address     │       │ status      │         │
│ created_at  │       │ total_amount│         │
└─────────────┘       │ created_at  │         │
         │            │ updated_at  │         │
         │            └─────────────┘         │
         │                   │                │
         │                   │                │
         │            ┌──────┴──────┐         │
         │            │             │         │
         │      ┌─────┴─────┐ ┌─────┴─────┐   │
         │      │OrderDetail│ │  Payment  │   │
         │      ├───────────┤ ├───────────┤   │
         │      │ id (PK)   │ │ id (PK)   │   │
         └──────┤ order_id  │ │ order_id  │   │
                │ product_id│ │ method    │   │
                │ quantity  │ │ amount    │   │
                │ unit_price│ │ payment_dt│   │
                │ subtotal  │ │ status    │   │
                │ created_at│ │ created_at│   │
                └───────────┘ └───────────┘   │
                            │        │        │
                            │        │        │
                            └────────┼────────┘
                                     │
                            ┌────────┴────────┐
                            │    Shipment     │
                            ├─────────────────┤
                            │ id (PK)         │
                            │ order_id (FK)   │
                            │ shipping_address│
                            │ tracking_number │
                            │ status          │
                            │ shipped_date    │
                            │ delivered_date  │
                            │ created_at      │
                            │ updated_at      │
                            └─────────────────┘

![diagramaER](diagramaER.png)

### Logical Model
# Main Entities:
Seller: Manages vendor/seller information

Customer: Stores buyer data for the marketplace

Product: Contains items available for sale

Category: Organizes products into thematic categories

Order: Records purchase transactions

Payment: Handles financial transaction information

Shipment: Manages product delivery process

Review: Stores customer opinions and ratings

Tag: Tag system for flexible product categorization

## Relationships and Constraints
# Main Relationships
Seller → Product: One to Many (one seller has many products)

Category → Product: One to Many (one category has many products)

Customer → Order: One to Many (one customer has many orders)

Order → OrderDetail: One to Many (one order has many details)

Product → OrderDetail: One to Many (one product appears in many details)

Product → Review: One to Many (one product has many reviews)

Customer → Review: One to Many (one customer makes many reviews)

Product ↔ Tag: Many to Many (through product_tag)

# Integrity Constraints
Unique keys on seller and customer emails

CHECK constraints on rating (1-5)

Default values on date fields

NOT NULL constraints on mandatory fields

Appropriate cascade deletion configuration

### 📘SITE MARKETPLACE
# Backend Documentation

The backend is built using Django with Django REST Framework following a modular application structure. The architecture supports:

Multi-database engine support (MySQL, PostgreSQL, MSSQL, Oracle)

RESTful API design with ViewSets

Modular app structure for scalability

CORS enabled for frontend integration

Environment-based configuration

![vsc](vsc.png)

# Core Configuratio

# Database Configuration (Multi-engine support)
DATABASE_CONFIGS = {
    'mysql': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': config('MYSQL_NAME'),
        'USER': config('MYSQL_USER'),
        'PASSWORD': config('MYSQL_PASSWORD'),
        'HOST': config('MYSQL_HOST'),
        'PORT': config('MYSQL_PORT'),
    },
    'postgresql': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('POSTGRES_NAME'),
        'USER': config('POSTGRES_USER'),
        'PASSWORD': config('POSTGRES_PASSWORD'),
        'HOST': config('POSTGRES_HOST'),
        'PORT': config('POSTGRES_PORT'),
    }
}

# Installed Apps
INSTALLED_APPS = [
    'rest_framework',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'corsheaders',
    'myapps.user',
    'myapps.product',
    'myapps.order',
    'myapps.payment',
    'myapps.shipping',
    'myapps.review',
]

# CORS Configuration
CORS_ORIGIN_ALLOW_ALL = True



