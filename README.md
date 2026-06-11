# 🛒 Grocery Store Backend — Django REST API

A production-deployed backend for an online grocery store built with Django REST Framework. Features JWT authentication, role-based access control, AWS S3 media storage, AWS RDS (PostgreSQL), and a live EC2 deployment.

🔗 **Live API:** `http://65.0.92.157/api/`

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Django 5 + Django REST Framework |
| Auth | SimpleJWT (access + refresh tokens) |
| Database | PostgreSQL (AWS RDS) |
| Media Storage | AWS S3 |
| Deployment | AWS EC2 + Nginx + Gunicorn |
| Language | Python |

---

## Features

### 🔐 Authentication & Roles
- JWT-based auth with access and refresh tokens
- Three roles: **Customer**, **Manager**, **Admin**
- Custom `permissions.py` for role-based route protection

### 🛍️ Customer
- Browse, search, filter, and sort products by category and price
- Manage cart and wishlist
- Apply promo codes at checkout with discount validation
- Checkout with real-time stock validation and stock reduction
- View full order history

### 🧑‍💼 Manager
- Full CRUD for categories and products
- Product image management via AWS S3
- Create and manage promo codes
- Sales reports — most/least sold products, filterable by category
- Low-stock alerts via Django signals

### 👑 Admin
- Promote users to Manager role

---

## Project Structure

```
grocery-backend/
├── manage.py
├── grocery_backend/
│   ├── settings.py          # Environment-based config
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── store/
│   ├── models.py            # DB schema
│   ├── serializers.py       # API validation & serialization
│   ├── views.py             # Business logic
│   ├── permissions.py       # Role-based access control
│   ├── urls.py              # Route definitions
│   ├── signals.py           # Low-stock alerts
│   └── admin.py
├── requirements.txt
└── grocery_store_db_uml.png  # Database schema diagram
```

---

## Database Schema

Entities: `User`, `Category`, `Product`, `ProductImage`, `CartItem`, `WishlistItem`, `Order`, `OrderItem`, `PromoCode`

UML diagram included in repo root: `grocery_store_db_uml.png`

---

## Getting Started

### Prerequisites
- Python 3.10+
- PostgreSQL
- AWS account (for S3 + RDS in production)

### Installation

```bash
git clone https://github.com/priyanshukr07/grocery-backend.git
cd grocery-backend
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file:

```env
# Django
SECRET_KEY=your_secret_key
ALLOWED_HOSTS=localhost,127.0.0.1

# PostgreSQL
DB_NAME=grocery_db
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=your_db_host
DB_PORT=5432

# AWS S3
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_STORAGE_BUCKET_NAME=your_bucket
AWS_REGION=ap-south-1
```

### Run Locally

```bash
python manage.py migrate
python manage.py runserver
```

API available at `http://localhost:8000/api/`

---

## Key API Endpoints

| Method | Endpoint | Role | Description |
|---|---|---|---|
| POST | `/api/auth/register/` | Public | Register as Customer |
| POST | `/api/auth/login/` | Public | Get JWT tokens |
| POST | `/api/auth/token/refresh/` | Public | Refresh access token |
| GET | `/api/products/` | All | Browse/search/filter products |
| POST | `/api/cart/` | Customer | Add to cart |
| POST | `/api/checkout/` | Customer | Place order with promo |
| GET | `/api/orders/` | Customer | Order history |
| POST | `/api/products/` | Manager | Create product |
| GET | `/api/reports/sales-by-product/` | Manager | Sales report |
| PATCH | `/api/users/:id/promote/` | Admin | Promote to Manager |

Full Postman collection included: `grocery_store_collection.json`

---

## Deployment Architecture

```
Client → Nginx (EC2) → Gunicorn → Django
                              ↓
                         AWS RDS (PostgreSQL)
                         AWS S3 (media files)
```

- EC2 instance running Ubuntu with Nginx as reverse proxy
- Gunicorn as the WSGI application server
- Static files served by Nginx, media files on S3
- Environment-based settings for local vs production

---

## Testing

```bash
python manage.py test
```

Unit and integration tests in `store/tests.py`.

---

## Author

**Priyanshu Kumar**
[LinkedIn](https://linkedin.com/in/priyanshukr07) · [GitHub](https://github.com/priyanshukr07)
