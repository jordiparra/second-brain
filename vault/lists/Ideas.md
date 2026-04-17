# AI Ideas

```base
filters:
  and:
    - category == "Ideas"
    - context == "ai"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Idea
  note.status:
    displayName: Status
views:
  - type: table
    name: AI Ideas
    order:
      - file.name
      - status
    sort:
      - property: status
        direction: ASC
      - property: file.name
        direction: ASC
    columnSize:
      file.name: 572

```
