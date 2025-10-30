# Frontend Integration Patterns

## Setup API Client
```
import Medusa from "@medusajs/medusa-js"

const client = new Medusa({
  baseUrl: "http://localhost:9000"
})
```

## JWT Token Management
```
export const tokenStorage = {
  setToken(token) { localStorage.setItem("jwt", token) },
  getToken() { return localStorage.getItem("jwt") },
  clear() { localStorage.removeItem("jwt") }
}
```

## Login
```
export async function login(email, password) {
  const r = await fetch("/store/auth/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email, password })
  })
  const data = await r.json()
  tokenStorage.setToken(data.token)
  return data.customer
}
```

## Products Hook
```
import { useState, useEffect } from "react"

export function useProducts(limit = 20) {
  const [products, setProducts] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    fetch(`/store/products?limit=${limit}`)
      .then(r => r.json())
      .then(data => {
        setProducts(data.products)
        setLoading(false)
      })
  }, [limit])

  return { products, loading }
}
```

## Cart Context
```
import { createContext, useContext, useState } from "react"

const CartContext = createContext()

export function CartProvider({ children }) {
  const [cart, setCart] = useState(null)

  async function addToCart(variantId, qty = 1) {
    const r = await fetch(`/store/carts/${cart.id}/line-items`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ variant_id: variantId, quantity: qty })
    })
    const data = await r.json()
    setCart(data.cart)
  }

  return (
    <CartContext.Provider value={{ cart, addToCart }}>
      {children}
    </CartContext.Provider>
  )
}

export function useCart() {
  return useContext(CartContext)
}
```
