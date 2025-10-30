# Medusa Hooks & Events Reference

## Order Events

### order.placed
Fired when order is successfully placed.

Use Cases:
- Send confirmation email
- Trigger fulfillment
- Create invoice
- Update inventory

### order.confirmed
Fired on order confirmation.

### order.canceled
Fired on order cancellation.

### order.completed
Fired on order fulfillment.

## Product Events

### product.created
Fired when product is created.

### product.updated
Fired when product is modified.

### product.deleted
Fired when product is deleted.

## Customer Events

### customer.created
Fired when new customer registers.

### customer.updated
Fired when customer profile is updated.

## Implementing Event Subscribers
```
class OrderService {
  constructor({ eventBusService, emailService }) {
    this.eventBusService = eventBusService
    this.emailService = emailService
    
    this.eventBusService_.subscribe("order.placed", async (data) => {
      await this.emailService_.send({
        to: data.email,
        template: "order-confirmation"
      })
    })
  }
}
```
