# Books

```base
filters:
  and:
    - category == "Books"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Title
  note.author:
    displayName: Author
  note.rating:
    displayName: Rating
  note.status:
    displayName: Status
  note.date-read:
    displayName: Read
views:
  - type: table
    name: All
    order:
      - file.name
      - author
      - rating
      - status
      - date-read
    columnSize:
      file.name: 280
      property.author: 180
      property.rating: 70
      property.status: 110
      property.date-read: 110
    sort:
      - property: date-read
        direction: DESC
  - type: table
    name: Read
    filters:
      and:
        - status == "read"
    order:
      - file.name
      - author
      - rating
      - date-read
    columnSize:
      file.name: 280
      property.author: 180
      property.rating: 70
      property.date-read: 110
    sort:
      - property: date-read
        direction: DESC
  - type: table
    name: Want to read
    filters:
      and:
        - status == "want-to-read"
    order:
      - file.name
      - author
    columnSize:
      file.name: 300
      property.author: 200
    sort:
      - property: file.name
        direction: ASC
  - type: table
    name: On hold
    filters:
      and:
        - status == "on-hold"
    order:
      - file.name
      - author
    columnSize:
      file.name: 300
      property.author: 200
    sort:
      - property: file.name
        direction: ASC
```
