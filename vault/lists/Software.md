# Software

```base
filters:
  and:
    - 'category == "Software"'
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.maker:
    displayName: Maker
  note.rating:
    displayName: Rating
views:
  - type: table
    name: All
    order:
      - file.name
      - maker
      - rating
    sort:
      - property: file.name
        direction: ASC
```
