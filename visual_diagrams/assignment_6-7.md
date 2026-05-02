```mermaid
sequenceDiagram
    actor Customer
    participant CI as Cart Interface
    participant CM as Cart Mgr
    participant PM as Product Mgr
    participant PI as Product Items
    participant WCS as White Crew Socks
    participant CIT as Cart Items

    Customer->>CI: Add white crew socks to cart
    CI->>CM: Add white crew socks to cart
    CM->>PM: Get white crew socks
    PM->>PI: Find product (white crew socks)
    PI->>WCS: Get product
    CM->>CIT: Add white crew socks to cart
    CIT->>CIT: Add white crew socks to cart
```
