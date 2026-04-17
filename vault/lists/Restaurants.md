# Restaurants

```base
filters:
  and:
    - category == "Restaurants"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.city:
    displayName: City
  note.cuisine:
    displayName: Cuisine
  note.price-range:
    displayName: Price range
  note.status:
    displayName: Status
views:
  - type: table
    name: Restaurants
    order:
      - file.name
      - city
      - price-range
      - status
    sort:
      - property: city
        direction: ASC
      - property: file.name
        direction: ASC
    columnSize:
      file.name: 346
      note.city: 121
      note.price-range: 119
      note.status: 110

```
