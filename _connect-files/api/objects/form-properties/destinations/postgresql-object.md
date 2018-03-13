---
content-type: "api-form"
form-type: "destination"
key: "destination-form-properties-postgresql-object"

title: "PostgreSQL"
description: "A PostgreSQL destination connection writes data to a PostgreSQL database and corresponds to the destination type = `postgres`."

object-attributes:
  - name: "host"
    type: "string"
    description: "The IP address or hostname of the database server."

  - name: "port"
    type: "integer"
    description: "The port of the database server."

  - name: "database"
    type: "string"
    description: "The name of the logical database to connect to."

  - name: "username"
    type: "string"
    description: "The username of the database user."

  - name: "password"
    type: "string"
    description: "The password for the user connecting to the database server. **Note**: This property will never be returned by the API, but it can be submitted when creating or modifying a connection."

  - name: "ssl"
    type: "boolean"
    description: "If `true`, SSL will be used to connect to the database."

examples:
  - code: |
      {
         "connection":{
            "host":"postgres.some-host.com",
            "port":5432,
            "database":"stitch",
            "username":"stitch_user",
            "password":"<PASSWORD>",
            "ssl":true
         }
      }
---