# Places

```base
filters:
  and:
    - 'category == "Places"'
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.city:
    displayName: City
  note.country:
    displayName: Country
  note.status:
    displayName: Status
views:
  - type: table
    name: Places
    order:
      - file.name
      - country
      - city
      - status
    columnSize:
      file.name: 250
      property.country: 130
      property.city: 130
      property.status: 100
    sort:
      - property: country
        direction: ASC
      - property: city
        direction: ASC
```
