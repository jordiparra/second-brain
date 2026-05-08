# All Projects

```base
filters:
  and:
    - 'category == "Projects"'
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Project
  note.context:
    displayName: Context
  note.status:
    displayName: Status
views:
  - type: table
    name: All
    order:
      - file.name
      - context
      - status
    sort:
      - property: context
        direction: ASC
      - property: file.name
        direction: ASC
  - type: table
    name: Work
    filters:
      and:
        - 'context == "work"'
    order:
      - file.name
      - status
    sort:
      - property: file.name
        direction: ASC
  - type: table
    name: Personal
    filters:
      and:
        - 'context == "personal"'
    order:
      - file.name
      - status
    sort:
      - property: file.name
        direction: ASC
```
