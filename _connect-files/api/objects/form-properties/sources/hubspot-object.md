---
content-type: "api-form"
form-type: "source"
key: "source-form-properties-hubspot-object"

title: "HubSpot"
description: "{{ api.form-properties.source-forms.hubspot.description }}"

object-attributes:
  - name: "frequency_in_minutes"
    type: "string"
    description: |
      {{ connect.common.attributes.frequency | replace: "[INTEGRATION]",form-property.title }}

  - name: "start_date"
    type: "string"
    description: "{{ connect.common.attributes.start-date }}"

examples: 
  - code: |
      {  
       "type":"platform.hubspot",
       "properties":{  
          "frequency_in_minutes":"30",
          "start_date":"2018-01-10T00:00:00Z"
        }
      }
---