# BOSH Release for neo4j

This release support both neo4j Community and Enterprise editions

#### Here is some deployment manifest examples:

* neo4j community example:
```yaml
jobs:
  - name: neo4j_z1
    instances: 1
    templates:
      - name: neo4j_community
        release: neo4j-hybris
    resource_pool: rp_z1
    [...]
```


* neo4j enterprise example:
```yaml
jobs:
  - name: neo4j_z1
    instances: 2
    templates:
      - name: neo4j_enterprise
        release: neo4j-hybris
    resource_pool: rp_z1
    properties:
      neo4j:
        server:
          database_mode: HA # set the neo4j in high availability mode
          index_offset: 0 # set the number of instances of previous jobs
        ha:
          initial_hosts:
            - 10.10.244.10
            - 10.10.244.11
            - 10.10.244.12
    [...]
  - name: neo4j_z2
    instances: 1
    templates:
      - name: neo4j_enterprise
        release: neo4j-hybris
    resource_pool: rp_z2
    properties:
      neo4j:
        server:
          database_mode: HA # set the neo4j in high availability mode
          index_offset: 2 # set the number of instances of previous jobs
        ha:
          initial_hosts:
            - 10.10.244.10
            - 10.10.244.11
            - 10.10.244.12
    [...]
```
