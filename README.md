# documentation

  flowchart TD                                                                                            ExtData[/"External Data Sources"/]

      subgraph onprem["On-Premises Infrastructure"]
          INPCR[("INPCR\n─\nOracle")]
          Talend[["Talend ETL"]]
          OMOP[("INPC OMOP Release\n─\nPostgres")]
          Registries[("Peel-off OMOP\nRegistries\n─\nPostgres")]
      end

      Analysts(["Data Analysts"])
      Deliveries[/"Dataset Deliveries"/]

      ExtData --> INPCR
      INPCR --> Talend
      INPCR -->|"some datasets"| Analysts
      Talend --> OMOP
      Talend --> Registries
      OMOP -->|"other datasets"| Analysts
      Analysts --> Deliveries
