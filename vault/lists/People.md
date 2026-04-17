# People

```base
filters:
  and:
    - category == "People"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.context:
    displayName: Context
  note.role:
    displayName: Role
views:
  - type: table
    name: People
    order:
      - file.name
      - context
      - role
    columnSize:
      file.name: 220
      property.context: 100
      property.role: 180
    sort:
      - property: file.name
        direction: ASC
```
