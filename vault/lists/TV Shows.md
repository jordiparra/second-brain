# TV Shows

```base
filters:
  and:
    - category == "TV Shows"
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Title
  note.rating:
    displayName: Rating
  note.year:
    displayName: Year
  note.status:
    displayName: Status
  note.date-watched:
    displayName: Watched
views:
  - type: table
    name: All
    order:
      - file.name
      - rating
      - year
      - date-watched
    sort:
      - property: date-watched
        direction: DESC
    columnSize:
      file.name: 423
      note.rating: 80
      note.year: 84
  - type: table
    name: Top rated
    filters:
      and:
        - rating >= 8
    order:
      - file.name
      - rating
      - year
      - date-watched
    sort:
      - property: rating
        direction: DESC
    columnSize:
      file.name: 300
      property.rating: 70
      property.year: 70
      property.date-watched: 110

```
