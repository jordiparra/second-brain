# Events

```base
filters:
  and:
    - category == "Events"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.context:
    displayName: Context
  note.loc:
    displayName: Location
  note.start:
    displayName: Start
  note.end:
    displayName: End
views:
  - type: table
    name: Events
    order:
      - file.name
      - context
      - loc
      - start
      - end
    sort:
      - property: start
        direction: DESC
    columnSize:
      file.name: 250
      note.context: 100
      note.loc: 150
      note.start: 110
      note.end: 110
```
