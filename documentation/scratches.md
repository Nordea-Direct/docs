
# Capital allocation between CFF and GAI


## Components
```mermaid
flowchart LR
  classDef changes stroke-width:4px
    
  subgraph CFF
    B(Brick)-- Internal java search for capital allocations -->C(Creditflow)
  end
  subgraph Kasper Server
    B-- POST REST/JSON -->K(Capital Allocation Service)
    K-- File --> KS(Fileshare)
  end 
  subgraph Mainframe Norway
    KS-- SFTP/file -->MN(Mainframe Norway)
  end
  subgraph Mainfraim Sweden 
    MN-- SFTP/file -->GL(GAI Landing Area)
    GL-- File -->G(Global Account Interface)
  end
  
  class B,K changes;             
```



## Sequence

```mermaid
sequenceDiagram
  participant B as Brick
  participant C as Creditflow    
  participant K as Capital Allocation Service  
  participant MN as Mainframe Norway
  participant MS as Mainframe Sweden
  participant GL as GAI Landing Area
  participant G as Global Account Interface
  Note right of B: cff-brk-capital-allocation
  B->>C: request capital allocation data
  Note right of B: Scheduled every last day of month
  B->>K: deliver capital allocation data
  K->>K: store file
  K->>MN: transfer file 
  MN->>MN: enrich with GAI metadata
  MN->>MS: transfer file 
  MS->>GL: transfer file
  GL->>G: process capital allocation data            
```

