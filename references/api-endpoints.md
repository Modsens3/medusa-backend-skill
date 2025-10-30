# API Reference

## Products
GET /store/products - List
GET /store/products/:id - Single

## Cart
POST /store/carts - Create
POST /store/carts/:id/line-items - Add
POST /store/carts/:id/complete - Checkout

## Orders
GET /store/orders - List
GET /store/orders/:id - Get

## Customers
POST /store/customers - Register
POST /store/auth/login - Login
