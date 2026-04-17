# Eurorack

```base
filters:
  and:
    - category == "Eurorack"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Module
views:
  - type: table
    name: Modules
    order:
      - file.name
    sort:
      - property: file.name
        direction: ASC
```
