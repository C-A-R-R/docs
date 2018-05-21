---
title: Replication Scheduling
permalink: /replication/replication-scheduling
redirect_from: /replication/replication-frequency
keywords: replicate, replication, replication frequency, frequency, anchor time, scheduling, schedule, interval, change replication time
tags: [replication]

summary: "Create a replication schedule for your integration's using Stitch's Replication Frequency and Anchor Time features."
type: "settings"
toc: true
weight: 1
---
{% include misc/data-files.html %}

[INTRO OF SOME TYPE]

- Note up here than this is for EXTRACTION, not LOADING.

How you create a replication schedule depends on your needs:

- **To kick off a replication job every minute or 30 minutes**, use the [Replication Frequency](#replication-frequency) setting.

- **To kick off replication jobs at defined intervals of an hour or more**, use [Anchor Scheduling](#anchor-scheduling).

---

## Replication Frequency

The Replication Frequency setting, found in the {{ app.page-names.int-settings }} page, defines how often Stitch will attempt to extract data from an integration.

For example: If set to **30 minutes**, Stitch will attempt to connect to and extract data from the integration every 30 minutes.

### Replication frequency intervals and scheduling {#frequency-intervals-scheduling}

When you define an integration's Replication Frequency, Stitch will schedule replication jobs based on the **start time** of the previous job.

If the previous job is still in progress when it would be time to start the next job, Stitch will wait until the next recurrence of the Replication Frequency to begin.

For example: A Facebook Ads integration has a Replication Frequency of 30 minutes. The Start and End Times for this integration's replication jobs might look like this:

| Job # | Start Time | End Time | Duration (minutes) |
|-------+------------+----------+--------------------|
| Job 1 | 12:00:00   | 12:42:00 | 42                 |
| Job 2 | 13:00:00   | 13:29:00 | 29                 |
| Job 3 | 13:30:00   | 14:03:00 | 33                 |
| Job 4 | 14:30:00   | ...      | ...                |


Notice Job 2's Start Time. Because Job 1 took 42 minutes to complete, or longer than the 30 minute Replication Frequency, Job 2 began at the next recurrence of the defined frequency. In this case, that was `13:00:00` instead of `12:30:00`.

---

## Anchor scheduling

Anchor scheduling uses an Anchor Time in conjunction with a Replication Frequency to create a replication schedule. Anchor scheduling allows you to establish more predictable replication and ensure that your downstream processes are using the most up-to-date data.

To use anchor scheduling, you'll need to:

- **Select a Replication Frequency** greater than an hour. One hour is the minimum frequency required to use anchor scheduling, as using an anchor time with a frequency less than an hour wouldn't affect an integration's replication schedule.
- **Define an Anchor Time**. An Anchor Time is the time that the Replication Frequency is "anchored" to, which Stitch will use to create a replication schedule.

{% capture loading-tip %}
As scheduling affects the time a replication job starts - **not the time to loaded data** - you should factor in time for loading the data when setting an Anchor Time. To get an idea of your integration's average loading times, use the [Loading Reports]({{ link.replication.loading-reports | prepend: site.baseurl }}).
{% endcapture %}

{% include tip.html content=loading-tip %}

### Create an anchored schedule

For example: If the Replication Frequency is set to **6 hours** and the Anchor Time is **9:00 AM** (EST), or **UTC 13:00**, Stitch will kick off a replication job every 6 hours starting at 9:00 AM (EST). The schedule for this integration would look like this:

| Job # | Start Time (EST) | Start Time (UTC) |
|-------+------------------+------------------|
| Job 1 | Mon 09:00:00     | Mon 13:00:00     |
| Job 2 | Mon 15:00:00     | Mon 19:00:00     |
| Job 3 | Mon 21:00:00     | Tues 01:00:00    |
| Job 4 | Tues 03:00:00    | Tues 07:00:00    |
| Job 5 | Tues 09:00:00    | Tues 13:00:00    |

If the previous job is still in progress when it would be time to start the next job, Stitch will wait until the next recurrence of the Replication Frequency to begin. See [the previous section for an example](#frequency-intervals-scheduling).

---

## Row Count Impact & Replication Frequency

Replication Frequency affects not only how often your integrations are set to replicate, but how much data is replicated from the integration over time.

Even though we'll send out notifications when you're nearing your row limit, we recommend taking the following into consideration before setting the Replication Frequency for an integration:

- **Replication Frequency applies to the entire integration,** not individual tables. This means all tables set to sync will replicate according to the set frequency.
- **The number of syncing tables in the integration.** While you can sync individual tables in database integrations and the [SaaS integrations that support whitelisting]({{ link.replication.syncing | prepend: site.baseurl | append: "#integrations-that-support-whitelisting" }}), **most SaaS integrations have all tables set to sync by default.** 

   SaaS integrations with high table counts like QuickBooks can result in a high number of replicated rows if set to replicate frequently.
- **The Replication Methods syncing tables use.** Because it can reduce latency and your row count, we recommend using Incremental Replication for database integration tables whenever possible. A high Replication Frequency (such as 30 minutes) and a large number of tables that use Full Table Replication can quickly use up your row count and possibly lead to overages.

   - **If some tables have to use Full Table Replication**, check out the [Different Frequencies for Full & Incrementally Replicating Tables](#different-frequencies-for-full--incrementally-replicating-tables) section below for a solution that can significantly reduce row usage.

   - **For info on the Replication Methods used by SaaS integration tables**, refer to the [**Schema section**]({{ site.baseurl }}/integrations/saas) on any integration's page.
- **How the integration replicates as a whole.** This typically applies to SaaS integrations and the querying strategies Stitch employs to extract data.

  For example: Every time a sync runs for [Facebook Ads]({{ site.baseurl }}/integrations/saas/facebook-ads#replication), **the past 28 days' worth of data will be replicated**. This is to account for changes that can be made during the 28 day attribution window on campaigns. Even though all tables in this integration use Incremental Replication, the number of replicated rows can still be huge because of the strategy Stitch uses to query.

  For more info on how SaaS integration data is queried/replicated, refer to the [Replication section]({{ site.baseurl }}/integrations/saas) in the SaaS integration docs.
- **The importance of completely fresh data.** If you don't need data to be constantly replicated or if tasks can still be completed with _slightly_ older data, consider setting integrations to replicate less frequently.

---

## Define Different Frequencies for Full & Incremental Tables {#different-frequencies-for-full--incrementally-replicating-tables}

{% capture workaround %}**This workaround only applies to [integrations that support whitelisting at the table level]({{ link.replication.syncing | prepend: site.baseurl | append: "#integrations-that-support-whitelisting" }}).**

<br><br>Additionally, while this will work for some SaaS integrations, some SaaS integration providers may not allow more than one API session to be open at a time. [NetSuite]({{ site.baseurl }}/integrations/saas/netsuite#create-netsuite-admin-user) is just such an example. We are unsure of the exact impact this may have on any API quotas imposed by the integration provider.
{% endcapture %}
{% include note.html content=workaround %}

To help keep row counts low, you can create two integrations: one to sync tables using Full Table Replication, the other to sync Incremental tables. You can then set each integration's Replication Frequency appropriately.

### Example

Let's say you have two tables in a PostgreSQL database that you want to sync: `files` and `orders`.

The `files` table contains 50,000 rows and uses Full Table Replication, while `orders` contains 100,000 rows and updates incrementally.

On an average day, 500 rows are added or updated to the `orders` table. For `files`, perhaps 10 rows are added or updated each day. It isn't necessary to have `files` replicate as often as `orders`.

To help keep your row count down, you could:

1. Create a PostgreSQL integration and name it `Postgres - Full Table` or something similar.
2. Only select tables using **Full Table Replication** to sync. In this case, that would be the `files` table.
3. Set the integration's Replication Frequency to every **24 hours**.

Then, to ensure the `orders` table is updated regularly:

1. Create an additional PostgreSQL integration and name it `Postgres - Incremental` or something similar.
2. Only select tables using **Incremental Replication** to sync. In this case, that would be the `orders` table.
3. Set the integration's Replication Frequency to every **30 minutes, 1 hour**, etc.
