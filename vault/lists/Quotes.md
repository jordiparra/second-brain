# Quotes

```base
filters:
  and:
    - category == "Quotes"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Quote
  note.attribution:
    displayName: By
  note.created:
    displayName: Date
views:
  - type: table
    name: All
    order:
      - file.name
      - attribution
      - created
    sort:
      - property: created
        direction: DESC
    columnSize:
      file.name: 454
      note.attribution: 100
      note.created: 110

```
