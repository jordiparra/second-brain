# Stocks

```base
filters:
  and:
    - category == "Stocks"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.ticker:
    displayName: Ticker
  note.status:
    displayName: Status
views:
  - type: table
    name: All
    order:
      - file.name
      - ticker
      - status
    columnSize:
      file.name: 280
      property.ticker: 120
      property.status: 120
    sort:
      - property: file.name
        direction: ASC
  - type: table
    name: Owned
    filters:
      and:
        - status == "owned"
    order:
      - file.name
      - ticker
    columnSize:
      file.name: 280
      property.ticker: 120
    sort:
      - property: file.name
        direction: ASC
  - type: table
    name: To watch
    filters:
      and:
        - status == "to-watch"
    order:
      - file.name
      - ticker
    columnSize:
      file.name: 280
      property.ticker: 120
    sort:
      - property: file.name
        direction: ASC
```
