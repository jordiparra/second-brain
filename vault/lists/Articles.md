# Articles

```base
filters:
  and:
    - 'category == "Articles"'
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Title
  note.author:
    displayName: Author
  note.topics:
    displayName: Topics
  note.created:
    displayName: Added
views:
  - type: table
    name: All
    order:
      - file.name
      - author
      - topics
      - created
    sort:
      - property: created
        direction: DESC
```
