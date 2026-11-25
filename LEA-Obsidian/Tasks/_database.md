---
notion-bases: true
schema:
  - id: file.name
    name: Name
    type: title
    visible: true
    width: 200
  - id: Worksesh
    name: Worksesh
    type: multiselect
    visible: true
    width: 140
  - id: Status
    name: Status
    type: select
    visible: true
    width: 120
  - id: Due
    name: Due
    type: date
    visible: true
    width: 160
  - id: Assigned
    name: Assigned
    type: multiselect
    visible: true
    width: 160
  - id: Projects
    name: Projects
    type: multiselect
    visible: true
    width: 160
views:
  - id: 8ec680ad-481d-440b-8bc8-82b8fd0b8767
    type: board
    filters: []
    sorts: []
    hiddenColumns: []
    columnWidths: {}
    pinnedColumnId:
    name: Board
    includeSubfolders: true
    boardHideNoValue: true
    activePills:
      - id: e76a1c21-103d-4e8d-827b-e7f5e97e1f7c
        columnId: Status
        operator: is_not
        value: Templates
        conjunction: and
askTemplateOnCreate: true
folderArrangement:
  enabled: true
  propertyIds:
    - Status
---
