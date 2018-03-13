---
content-type: "api-endpoint"
endpoint: "sessions"
key: "create-a-session"
version: "3"
order: 1


title: "Create a session"
method: "post"
short-url: |
  /v{{ object.version }}{{ object.endpoint-url }}/ephemeral
full-url: |
  {{ page.api-base-url }}{{ endpoint.short-url | flatify }}
description: "Creates a session in the Stitch web application. The session will be for the user for whom the API access token was created."

returns: "A [Session object]({{ page.anchors.core-objects.sessions.object }})."

examples:
  - type: "request"
    language: ""
    code: |
      curl -X {{ endpoint.method | upcase }} {{ endpoint.full-url | flatify | strip_newlines }}
           -H "Authorization: Bearer <ACCESS_TOKEN>" 
           -H "Content-Type: application/json"

  - type: "response"
    language: "json"
    code: |
      {
        "ephemeral_token":"<EPHEMERAL_TOKEN>"
      }

---