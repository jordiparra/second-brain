# Session Logs

```base
filters:
  and:
    - category == "Session Logs"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Session
  note.tags:
    displayName: Tags
views:
  - type: table
    name: All
    order:
      - file.name
      - tags
    sort:
      - property: file.name
        direction: DESC
    columnSize:
      file.name: 350
      note.tags: 300
```
