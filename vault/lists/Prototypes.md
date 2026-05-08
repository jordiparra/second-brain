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
  note.project:
    displayName: Project
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
      - project
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
      - project
      - status
      - url
    sort:
      - property: file.name
        direction: ASC
```
