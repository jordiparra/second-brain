# Products

```base
filters:
  and:
    - category == "Products"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.type:
    displayName: Type
  note.status:
    displayName: Status
views:
  - type: table
    name: All
    order:
      - file.name
      - type
      - status
    columnSize:
      file.name: 300
      property.type: 130
      property.status: 100
    sort:
      - property: type
        direction: ASC
  - type: table
    name: Want
    filters:
      and:
        - status == "want"
    order:
      - file.name
      - type
    columnSize:
      file.name: 300
      property.type: 150
    sort:
      - property: type
        direction: ASC
  - type: table
    name: Owned
    filters:
      and:
        - status == "owned"
    order:
      - file.name
      - type
    columnSize:
      file.name: 300
      property.type: 150
    sort:
      - property: file.name
        direction: ASC
```
