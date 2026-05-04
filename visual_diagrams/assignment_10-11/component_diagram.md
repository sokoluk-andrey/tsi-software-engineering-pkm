### Main Component Diagram
```mermaid
flowchart LR
    Entities[[Entities]]
    Boundaries[[Boundaries]]
    Control[[Control]]

    Entities -.-> Control
    Control -.-> Boundaries
```


### Entities Package Component Diagram
```mermaid
flowchart TB
    CC_spec[["CartCollection «spec»"]]
    CC_body[["CartCollection «body»"]]
    CI_spec[["CartItem «spec»"]]
    CI_body[["CartItem «body»"]]
    PC_spec[["ProductCollection «spec»"]]
    PC_body[["ProductCollection «body»"]]
    PI_spec[["ProductItem «spec»"]]
    PI_body[["ProductItem «body»"]]

    CC_body -.-> CC_spec
    CI_body -.-> CI_spec
    PC_body -.-> PC_spec
    PI_body -.-> PI_spec

    CI_spec -.-> CC_spec
    PI_spec -.-> PC_spec
    PI_spec -.-> CI_spec
```


### Control Package Component Diagram
```mermaid
flowchart LR
    CM_spec[["CartMgr «spec»"]]
    CM_body[["CartMgr «body»"]]
    PM_spec[["ProductMgr «spec»"]]
    PM_body[["ProductMgr «body»"]]

    CM_body -.-> CM_spec
    PM_body -.-> PM_spec
    PM_spec -.-> CM_spec
```


### Boundaries Package Component Diagram
```mermaid
flowchart LR
    CIF_spec[["CartInterface «spec»"]]
    CIF_body[["CartInterface «body»"]]

    CIF_body -.-> CIF_spec
```


### System Component Diagram
```mermaid
flowchart TB
    Main[["MainProgram"]]

    subgraph Boundaries
        CIF_spec[["CartInterface «spec»"]]
        CIF_body[["CartInterface «body»"]]
    end

    subgraph Control
        CM_spec[["CartMgr «spec»"]]
        CM_body[["CartMgr «body»"]]
        PM_spec[["ProductMgr «spec»"]]
        PM_body[["ProductMgr «body»"]]
    end

    subgraph Entities
        CC_spec[["CartCollection «spec»"]]
        CC_body[["CartCollection «body»"]]
        CI_spec[["CartItem «spec»"]]
        CI_body[["CartItem «body»"]]
        PC_spec[["ProductCollection «spec»"]]
        PC_body[["ProductCollection «body»"]]
        PI_spec[["ProductItem «spec»"]]
        PI_body[["ProductItem «body»"]]
    end

    %% Body to spec dependencies
    CIF_body -.-> CIF_spec
    CM_body -.-> CM_spec
    PM_body -.-> PM_spec
    CC_body -.-> CC_spec
    CI_body -.-> CI_spec
    PC_body -.-> PC_spec
    PI_body -.-> PI_spec

    %% Intra-package dependencies
    CI_spec -.-> CC_spec
    PI_spec -.-> PC_spec
    PI_spec -.-> CI_spec
    PM_spec -.-> CM_spec

    %% Cross-package dependencies
    PC_spec -.-> PM_spec
    CC_spec -.-> CM_spec
    CM_spec -.-> CIF_spec
    CIF_spec -.-> Main
```


### Component Diagrams for the Shopping Cart System

### What Was Done
Built five component diagrams modeling the deployment view of the shopping cart system: a Main Component diagram showing the three packages (Entities, Boundaries, Control) with their dependencies, three per-package diagrams showing the components inside each package (with package specifications and package bodies) and a System Component diagram showing all components together with the MainProgram and the full network of dependencies between them.

### Mermaid.js Steps
Used Mermaid's flowchart syntax with the subroutine shape [[...]] to approximate UML component boxes, and dashed arrows -.-> to represent UML dependency relationships. The «spec» and «body» labels were added inline to distinguish package specifications from package bodies. For the System Component diagram, subgraph blocks were used to visually group components into their packages.

### Why Flowchart Instead of Component Diagram
Mermaid does not natively support UML component diagrams. As a workaround, I used flowchart with subroutine-shaped nodes (which render with double vertical lines, similar to UML components) and dashed dependency arrows. Subgraphs represent the component packages. This reproduces the visual structure and dependency network of a UML component diagram.
