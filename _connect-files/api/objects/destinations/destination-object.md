---
content-type: "api-object"
endpoint: "destinations"

title: "Destination"
description: "Destinations are the data warehouses into which Stitch writes data."
endpoint-url: "/destinations"
version: "3"

object-attributes:
  - name: "id"
    type: "integer"
    description: "A unique identifier for this destination."

  - name: "connection"
    type: "destination form properties object"
    url: "{{ page.anchors.form-properties.destination-forms.section }}"
    description: |
      Parameters for connecting to the destination, excluding any sensitive credentials.

      The parameters must adhere to the `type` of destination.

  - name: "created_at"
    type: "timestamp"
    description: "The time at which the destination object was created."

  - name: "last_check"
    type: "connection check object"
    url: "{{ page.anchors.data-structures.connection-checks }}"
    description: "The status and results of the most recent check run for this destination connection."

  - name: "type"
    type: "string"
    description: |
      The destination type. Must be one of: 

      - `redshift`
      - `postgres`
      - `snowflake`

  - name: "updated_at"
    type: "timestamp"
    description: "The time at which the destination object was last updated."
---