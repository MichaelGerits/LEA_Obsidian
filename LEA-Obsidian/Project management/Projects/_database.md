---
notion-bases: true
schema:
  - id: file.name
    name: Project
    type: title
    visible: true
    width: 200
  - id: Blocking
    name: Blocking
    type: multiselect
    visible: true
    width: 140
  - id: summary
    name: summary
    type: text
    visible: false
    width: 260
  - id: Eng hours
    name: Eng hours
    type: number
    visible: true
    width: 100
  - id: Tasks
    name: Tasks
    type: multiselect
    visible: true
    width: 180
  - id: Departement
    name: Departement
    type: multiselect
    visible: true
    width: 140
  - id: Status
    name: Status
    type: select
    visible: true
    width: 120
  - id: Priority
    name: Priority
    type: select
    visible: true
    width: 100
  - id: Blocked by
    name: Blocked by
    type: multiselect
    visible: true
    width: 140
  - id: Timeline
    name: Timeline
    type: text
    visible: true
    width: 140
views:
  - id: default
    type: table
    filters: []
    sorts: []
    hiddenColumns: []
    columnWidths: {}
---
