---
notion-bases: true
schema:
  - id: file.name
    name: Content name
    type: title
    visible: true
    width: 200
  - id: Tasks
    name: Tasks
    type: multiselect
    visible: true
    width: 140
  - id: When
    name: When
    type: date
    visible: true
    width: 160
  - id: Department
    name: Department
    type: multiselect
    visible: true
    width: 140
  - id: event type
    name: event type
    type: select
    visible: true
    width: 140
  - id: Place
    name: Place
    type: text
    visible: true
    width: 120
  - id: attendees
    name: attendees
    type: multiselect
    visible: true
    width: 180
views:
  - id: ad78dacc-1173-4847-add7-5f9c5d27c5c6
    type: calendar
    filters: []
    sorts: []
    hiddenColumns:
      - When
      - file.name
      - Place
      - Tasks
    columnWidths: {}
    pinnedColumnId:
    name: Calendar
    calendarDateField: When
    includeSubfolders: true
folderArrangement:
  enabled: true
  propertyIds:
    - event type
askTemplateOnCreate: true
templateFolder: Calendar/Templates
---
