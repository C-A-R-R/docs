---
title: Amazon PostgreSQL RDS
keywords: amazon, amazon rds, rds, relational database services, database integration, etl rds, rds etl
tags: [database_integrations]
permalink: /integrations/databases/amazon-rds-postgresql
summary: "Connect and replicate data from your Amazon PostgreSQL RDS using Stitch's PostgreSQL integration."
microsites:
  - title: "{{ page.display_name }} to Postgres"
    url: "http://postgres.topostges.com/"

# -------------------------- #
#     Integration Details    #
# -------------------------- #

name: "postgresql-rds"
display_name: "PostgreSQL RDS"
author: "Stitch"
author-url: "https://www.stitchdata.com"

# -------------------------- #
#       Stitch Details       #
# -------------------------- #

status: "Released"
certified: true

frequency: "30 minutes"
tier: "Free"
port: 5432
db-type: "postgres"
icon: /images/integrations/icons/postgres-rds.svg

versions: "9.3+"
ssh: true
ssl: true
sync-views: true
whitelist:
  tables: true
  columns: true

setup-name: "PostgreSQL"

# -------------------------- #
#      Setup Requirements    #
# -------------------------- #

notice: |
  This guide describes how to connect {{ integration.display_name }} **as an input data source.**

  If you want to connect a {{ integration.display_name }} instance as a **destination**, refer to the [Connecting an {{ integration.display_name }} Destination guide]({{ link.destinations.setup.postgres-rds | prepend: site.baseurl }}).

requirements-list:
  - item: |
      **Permissions in Amazon Web Services (AWS) that allow you to**:

        - Create/manage Security Groups, which is required to whitelist Stitch's IP addresses.
        - View database details, which is required for retrieving the database's connection details.
  - item: "**Permissions in {{ integration.display_name }} that allow you to create/manage users.** This is required to create the Stitch database user."
  - item: |
      **To verify if the database is a read replica, or follower**. While we always recommend connecting a replica over a production database, this also means you may need to verify some of its settings - specifically the `standby` settings - before connecting it to Stitch.

      On occasion, [the default values for the `standby` settings can prevent Stitch from successfully completing queries]({{ link.troubleshooting.postgres-hot-standby | prepend: site.baseurl }}), resulting in slow, intermittent replication. This is usually only an issue during historical syncs or when replicating large amounts of data (ex: a large table using Full Table Replication).

      If you find that the `hot_standby` setting is `on`, proactively increasing the following settings from 30 seconds to 8-12 hours can help prevent this issue:

      - `max_standby_archive_delay`
      - `max_standby_streaming_delay`

      After the initial historical sync completes, you can typically decrease these settings again.

      For an official explanation of these settings, [check out the Postgres docs](https://www.postgresql.org/docs/9.0/static/runtime-config-wal.html#GUC-MAX-STANDBY-ARCHIVE-DELAY).

# -------------------------- #
#     Setup Instructions     #
# -------------------------- #

setup-steps:
  - title: "whitelist stitch ips"

  - title: "retrieve public key"

  - title: "create linux user"

  - title: "Create a Stitch database user"
    anchor: "create-a-database-user"
    content: |
      Next, you'll create a dedicated database user for Stitch. This will ensure Stitch is visible in any logs or audits, and allow you to maintain your privilege hierarchy.

      {% include integrations/templates/create-database-user-tabs.html %}

  - title: "Locate RDS connection details in AWS"
    anchor: "locating-rds-database-details"
    content: |

      {% include shared/aws-connection-details.html %}

  - title: "connect stitch"

  - title: "replication frequency"

  - title: "sync data"
---
{% assign integration = page %}
{% include misc/data-files.html %}
