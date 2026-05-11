# documentation

```mermaid
  flowchart TD
      ExtData[/"IHIE"/]

      subgraph onprem["On-Premises Infrastructure"]
          INPCR[("INPCR\n─\nOracle")]
          Talend[["Talend ETL"]]
          OMOP[("INPC OMOP Release\n─\nPostgres")]
          Registries[("Peel-off OMOP\nRegistries\n─\nPostgres")]
      end

      Deliveries[/"Dataset Deliveries"/]

      ExtData --> INPCR
      INPCR --> Talend
      INPCR --> Deliveries
      Talend --> OMOP
      Talend --> Registries
      OMOP --> Deliveries
      Registries --> Deliveries
```

```mermaid
  flowchart TD
      ExtData[/"IHIE"/]
      subgraph onprem["On-Premises Infrastructure"]
          INPCR[("INPCR\n─\nOracle")]
          Talend[["Talend ETL"]]
          OMOP_PG[("INPC OMOP Release\n─\nPostgres")]
          Registries[("Peel-off OMOP\nRegistries\n─\nPostgres")]
      end

      subgraph snowflake["Snowflake"]
          OMOP_SF[("INPC OMOP Release\n─\nSnowflake")]
      end

      Deliveries[/"Dataset Deliveries"/]

      ExtData --> INPCR
      INPCR --> Talend
      INPCR --> Deliveries
      Talend --> OMOP_PG
      Talend --> Registries
      OMOP_PG -.->|"Manual Load"| OMOP_SF
      OMOP_SF -->|"Faster generation of OMOP datasets made possible in this phase"| Deliveries
      Registries --> Deliveries

      classDef newNode fill:#d4edda,stroke:#28a745,color:#155724
      class OMOP_SF newNode
```

```mermaid
  flowchart TD
      ExtData[/"IHIE"/]                                                            
      subgraph onprem["On-Premises Infrastructure"]
          INPCR[("INPCR\n─\nOracle")]
      end                                                                                           
      subgraph snowflake["Snowflake"]
          INPCR_SF[("INPCR\n─\nSnowflake")]
          dbt[["dbt"]]
          OMOP_SF[("INPC OMOP Release\n─\nSnowflake")]
          Registries_SF[("Peel-off OMOP\nRegistries\n─\nSnowflake")]
      end

      Deliveries[/"Dataset Deliveries"/]

      ExtData --> INPCR
      INPCR --> Deliveries
      INPCR -.->|"Manual Load"| INPCR_SF
      INPCR_SF -->|"Faster transformation to OMOP made possible in this phase"| dbt
      dbt --> OMOP_SF
      dbt --> Registries_SF
      OMOP_SF --> Deliveries
      Registries_SF --> Deliveries

      classDef newNode fill:#d4edda,stroke:#28a745,color:#155724
      class INPCR_SF,dbt,Registries_SF newNode
```
