# Trips

```base
filters:
  and:
    - category == "Trips"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.destination:
    displayName: Destination
  note.start:
    displayName: Start
  note.end:
    displayName: End
  note.status:
    displayName: Status
views:
  - type: table
    name: Trips
    order:
      - file.name
      - destination
      - start
      - end
      - status
    sort:
      - property: start
        direction: DESC
    columnSize:
      file.name: 250
      note.destination: 150
      note.start: 110
      note.end: 110
      note.status: 100
```
