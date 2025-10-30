# Medusa Store API Endpoints Reference

## Products API

### List Products
GET /store/products?limit=20&offset=0

### Get Single Product
GET /store/products/:id

## Cart API

### Create Cart
POST /store/carts
Body: { "region_id": "reg_123" }

### Add Item to Cart
POST /store/carts/:id/line-items
Body: { "variant_id": "variant_123", "quantity": 1 }

### Remove Item
DELETE /store/carts/:id/line-items/:item_id

### Complete Cart
POST /store/carts/:id/complete

## Orders API

### List Orders (Authenticated)
GET /store/orders
Headers: Authorization: Bearer {token}

### Get Order
GET /store/orders/:id

## Customers API

### Register
POST /store/customers
Body: { "email": "user@example.com", "password": "pwd", "first_name": "John" }

### Login
POST /store/auth/login
Body: { "email": "user@example.com", "password": "pwd" }

### Get Current Customer
GET /store/customers/me
Headers: Authorization: Bearer {token}
