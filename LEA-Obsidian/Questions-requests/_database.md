---
notion-bases: true
schema:
  - id: Assigned to
    name: Assigned to
    type: multiselect
    visible: true
    width: 160
  - id: Type
    name: Type
    type: select
    visible: true
    width: 120
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
  - id: Created time
    name: Created time
    type: date
    visible: true
    width: 160
views:
  - id: f23b97e1-c48d-4f4d-b923-922249671e38
    type: board
    filters: []
    sorts: []
    hiddenColumns:
      - Type
      - Assigned to
    columnWidths: {}
    pinnedColumnId:
    name: Board
    includeSubfolders: true
    cardTitleField: Status
    groupByColumnId: Status
    activePills:
      - id: b6aec7b6-e2d2-4237-b97e-353493c6a2f5
        columnId: Status
        operator: is_not
        value: Templates
        conjunction: and
    boardHideNoValue: true
    conditionalFormats:
      - id: d91d99b3-9ec8-48e0-834c-ba20ff4ecfff
        columnId: Status
        operator: is
        value: Open
        bgColor: "#5480bb"
        textColor: ""
      - id: 97306129-9b73-4049-856a-125e3571b715
        columnId: Status
        operator: is
        value: Resolved
        bgColor: "#3dd171"
        textColor: "#f3f1f1"
      - id: 3b40c03b-1c63-42e9-b1db-4f4c1511ed39
        columnId: Status
        operator: is
        value: Won’t fix
        bgColor: "#d04343"
        textColor: "#ffffff"
      - id: c6a41ebc-25e4-4046-ac55-08a695dba1a3
        columnId: Status
        operator: is
        value: In review
        bgColor: "#f7d169"
        textColor: "#ffffff"
folderArrangement:
  enabled: true
  propertyIds:
    - Status
askTemplateOnCreate: true
templateFolder: Questions-requests/Templates
---
