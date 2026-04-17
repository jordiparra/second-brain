# Prototypes

```base
filters:
  and:
    - category == "Prototypes"
properties:
  file.name:
    displayName: Prototype
  note.context:
    displayName: Context
  note.area:
    displayName: Area
  note.status:
    displayName: Status
  note.url:
    displayName: URL
views:
  - type: table
    name: All
    order:
      - file.name
      - context
      - area
      - status
      - url
    sort:
      - property: file.name
        direction: ASC
  - type: table
    name: Work
    filters:
      and:
        - context == "work"
    order:
      - file.name
      - area
      - status
      - url
    sort:
      - property: file.name
        direction: ASC
```
