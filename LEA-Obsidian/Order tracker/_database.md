---
notion-bases: true
schema:
  - id: file.name
    name: Product Name
    type: title
    visible: true
    width: 200
  - id: URL
    name: URL
    type: text
    visible: true
    width: 160
  - id: Status
    name: Status
    type: select
    visible: true
    width: 120
    options:
      - value: ordered
      - value: arrived
      - value: on its way
      - value: not ordered
  - id: Cost (Unit)
    name: Price per unit
    type: number
    visible: true
    width: 120
  - id: amount
    name: Amount
    type: number
    visible: true
    width: 100
  - id: total
    name: Total price
    type: number
    visible: true
    width: 100
    formula: property.Cost (Unit) * property.amount
  - id: Category
    name: Category
    type: select
    visible: true
    width: 120
    options:
      - value: electronics
      - value: material
      - value: software
      - value: event
      - value: props
views:
  - id: 73718cd1-4dba-42ec-9d5c-ba9a113eae8f
    type: list
    filters: []
    sorts: []
    hiddenColumns: []
    columnWidths: {}
    pinnedColumnId:
    name: List
    includeSubfolders: true
    activePills: []
folderArrangement:
  enabled: true
  propertyIds:
    - Status
askTemplateOnCreate: true
templateFolder: Order tracker/Templates
---
