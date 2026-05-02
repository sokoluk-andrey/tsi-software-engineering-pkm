```mermaid
flowchart LR
    Customer((Customer))
    CreditSystem((Credit System))
    WarehouseManager((Warehouse Manager))
    ShippingService((Shipping Service))
    PurchasingManager((Purchasing Manager))

    subgraph System [E-Business System]
        UC1([Add Item to Shopping Cart])
        UC2([View Shopping Cart])
        UC3([View Details of Items])
        UC4([Purchase Items in Shopping Cart])
        UC5([Remove Item from Shopping Cart])
        UC6([Browse Items for Sale])
        UC7([Provide Feedback])
        UC8([Stock Inventory])
        UC9([Return Item to Stock])
        UC10([Ship Order])
        UC11([Add New Item for Sale])
        UC12([Remove Item for Sale])
        UC13([Purchase Inventory])
    end

    Customer --> UC1
    Customer --> UC2
    Customer --> UC3
    Customer --> UC4
    Customer --> UC5
    Customer --> UC6
    Customer --> UC7

    UC4 --> CreditSystem
    UC10 --> ShippingService

    WarehouseManager --> UC8
    WarehouseManager --> UC9
    WarehouseManager --> UC10

    PurchasingManager --> UC11
    PurchasingManager --> UC12
    PurchasingManager --> UC13
```

### Use Case Diagram for E-Business System
### What Was Done
Built a UML use case diagram modeling the order-processing e-business system. The diagram includes 13 system use cases (Add Item to Shopping Cart, View Shopping Cart, Purchase Items, Stock Inventory, Ship Order, Purchase Inventory, etc.) enclosed in a system boundary and 5 actors (Customer, Credit System, Warehouse Manager, Shipping Service, Purchasing Manager) with associations connecting them to the relevant use cases.

### Mermaid.js Steps
Used Mermaid's flowchart LR syntax with stadium-shaped nodes ([...]) to approximate the oval shape of UML use cases, and circle nodes (( )) to represent actors. The system boundary was drawn using a subgraph block and associations were added as directed arrows between actors and use cases.

### Why Flowchart Instead of Use Case Diagram
Mermaid does not natively support UML use case diagrams. As a workaround, I used flowchart LR with stadium shapes for use cases and circles for actors, wrapping the use cases in a subgraph to represent the system boundary. This reproduces the visual structure and associations of a UML use case diagram.

### Notation Limitation
Mermaid's circle nodes do not render as the traditional UML stick-figure actor symbol. This is a limitation of Mermaid's flowchart syntax — there is no built-in actor shape outside of sequence diagrams.

### Tool Choice
I chose Mermaid.js because I had already used it to build the architecture sequence diagram for the selected functionality per Ilin Maksim's task requirements.
