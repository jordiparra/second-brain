# Notes

```base
filters:
  and:
    - 'category == "Notes"'
    - '!file.name.contains("Template")'
properties:
  file.name:
    displayName: Name
  note.context:
    displayName: Context
views:
  - type: table
    name: All
    order:
      - file.name
      - context
    sort:
      - property: context
        direction: ASC
      - property: file.name
        direction: ASC
```
