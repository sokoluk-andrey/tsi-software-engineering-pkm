### Main Class Diagram (Packages)
```mermaid
classDiagram
    class Entities {
        <>
    }
    class Boundaries {
        <>
    }
    class Control {
        <>
    }
```

### Add Item to Shopping Cart — All Classes with Stereotypes
```mermaid
classDiagram
    class CartInterface {
        <<Boundary>>
    }
    class CartMgr {
        <<Control>>
    }
    class ProductMgr {
        <<Control>>
    }
    class Product {
        <<Entity>>
    }
    class ProductCollection {
        <<Entity>>
    }
    class CartCollection {
        <<Entity>>
    }
    class CartItem {
        <<Entity>>
    }
```

### Boundaries Package — Main Class Diagram
```mermaid
classDiagram
    class CartInterface {
        <<Boundary>>
    }
```

### Entities Package — Main Class Diagram
```mermaid
classDiagram
    class CartCollection {
        <<Entity>>
    }
    class Product {
        <<Entity>>
    }
    class ProductCollection {
        <<Entity>>
    }
    class CartItem {
        <<Entity>>
    }
```

###  Control Package — Main Class Diagram
```mermaid
classDiagram
    class CartMgr {
        <<Control>>
    }
    class ProductMgr {
        <<Control>>
    }
```
