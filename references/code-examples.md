# Medusa Code Examples

## Example 1: Product Detail Page
```
import { useEffect, useState } from "react"

export function ProductPage({ id }) {
  const [product, setProduct] = useState(null)
  const [variant, setVariant] = useState(null)

  useEffect(() => {
    fetch(`http://localhost:9000/store/products/${id}`)
      .then(r => r.json())
      .then(data => {
        setProduct(data.product)
        setVariant(data.product.variants[0])
      })
  }, [id])

  if (!product) return <div>Loading...</div>

  return (
    <div>
      <img src={product.thumbnail} />
      <h1>{product.title}</h1>
      {product.variants.map(v => (
        <button key={v.id} onClick={() => setVariant(v)}>
          {v.title}
        </button>
      ))}
      <p>${(variant?.prices[0]?.amount / 100).toFixed(2)}</p>
      <button>Add to Cart</button>
    </div>
  )
}
```

## Example 2: Product Search
```
import { useState, useEffect } from "react"

export function ShopPage() {
  const [products, setProducts] = useState([])
  const [search, setSearch] = useState("")

  useEffect(() => {
    const timer = setTimeout(() => {
      let url = `http://localhost:9000/store/products`
      if (search) url += `?q=${search}`
      
      fetch(url)
        .then(r => r.json())
        .then(d => setProducts(d.products))
    }, 500)

    return () => clearTimeout(timer)
  }, [search])

  return (
    <div>
      <input
        placeholder="Search..."
        value={search}
        onChange={e => setSearch(e.target.value)}
      />
      <div>
        {products.map(p => (
          <div key={p.id}>
            <img src={p.thumbnail} />
            <h3>{p.title}</h3>
          </div>
        ))}
      </div>
    </div>
  )
}
```

## Example 3: Checkout
```
export function CheckoutPage({ cart }) {
  const [addr, setAddr] = useState({
    first_name: "",
    last_name: "",
    address_1: "",
    city: ""
  })

  async function checkout() {
    await fetch(`/store/carts/${cart.id}/shipping-address`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(addr)
    })

    const r = await fetch(`/store/carts/${cart.id}/complete`, {
      method: "POST"
    })

    const data = await r.json()
    alert(`Order ${data.order.display_id} placed!`)
  }

  return (
    <div>
      <input placeholder="First Name" onChange={e => setAddr({...addr, first_name: e.target.value})} />
      <input placeholder="Last Name" onChange={e => setAddr({...addr, last_name: e.target.value})} />
      <input placeholder="Address" onChange={e => setAddr({...addr, address_1: e.target.value})} />
      <input placeholder="City" onChange={e => setAddr({...addr, city: e.target.value})} />
      <button onClick={checkout}>Place Order</button>
    </div>
  )
}
```

## Example 4: Backend Event
```
export default class OrderPlacedSubscriber {
  constructor({ eventBusService, emailService }) {
    this.bus = eventBusService
    this.email = emailService
  }

  attach(bus) {
    bus.subscribe("order.placed", this.onOrder.bind(this))
  }

  async onOrder(order) {
    await this.email.send({
      to: order.email,
      template: "order-confirmation",
      data: { id: order.display_id }
    })
  }
}
```
