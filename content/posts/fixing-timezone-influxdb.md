---
title: "Fixing Timezone Issues in InfluxDB for IoT Time-Series Data"
date: 2026-03-21
draft: false
description: "All things I did for fixing timezone issue"
tags: ["timezone", "influxdb"]
---

# **Fixing Timezone Issues in InfluxDB for IoT Time-Series Data**

## **Introduction**

If you're building a platform that displays time-series data to users across different timezones, you've probably run into subtle but frustrating display bugs which was data appearing on the wrong day, graphs shifting after a certain hour, or timestamps being off by hours. We hit exactly these issues on our IoT device management platform, where devices send data to InfluxDB at regular intervals. Here's what went wrong and how I fixed it.

## **The Setup**

We run a platform that manages IoT devices. Each device sends telemetry data to a server at regular intervals, and we store it in InfluxDB which is a natural fit for time-series data. Our users are in IST (Indian Standard Time, UTC+5:30), so all graphs and dashboards are expected to display data aligned to IST days.

## **The Problems**

We noticed two distinct issues in our UI:

### **Problem 1 - Timestamps showing one day ahead**

When querying daily aggregated data, the timestamp returned for a given day's bucket was showing the *next* day's date. For example, data for March 5th was being labeled as March 6th.

### **Problem 2 - Daily graphs only populating after 6:30 PM**

On the UI, a new day's graph start receiving data from 6:30 PM IST - instead of at IST midnight (00:00).

## **The Root Cause: InfluxDB Defaults to UTC**

Both issues stem from the same underlying behaviour: **InfluxDB uses UTC as its default timezone for all operations**, including the `window()` function used for time-based aggregations.

When you use `window(every: 1d)` in a Flux query, InfluxDB splits the data into daily buckets starting at **UTC 00:00**. Since IST is UTC+5:30, IST midnight actually corresponds to **18:30 UTC the previous day**. This means:

- InfluxDB's "day boundary" is 5 hours 30 minutes off from what our users expect.
- Data that falls between 18:30 UTC and 23:59 UTC, which is 00:00 to 05:29 IST which gets bucketed into the *next* UTC day.

This explained why graphs only showed data after 6:30 PM IST (when the UTC day "caught up" to IST) and why timestamps were shifted forward.

The second contributing factor is the `timeSrc` parameter in the `window()` function. By default, `timeSrc` is set to `_stop`, meaning each bucket is **labeled with its end timestamp** rather than its start. Since the end of a day's bucket is technically the start of the next day (e.g., `2024-03-06T00:00:00Z`), the data appeared to belong to the following day.

---

**The Fix**

Two targeted changes resolved both issues:

**Fix 1 - Use `timeSrc: v._start` in the window function**

Changing `timeSrc` to `v._start` ensures each bucket is labeled with its *start* timestamp instead of its end, so data for March 5th is correctly labeled as March 5th.

```flux
window(every: 1d, timeSrc: "_start")
```

**Fix 2 - Pass the client's timezone to InfluxDB**

Using InfluxDB's `timezone` package, we set the query timezone dynamically based on the client's locale. For IST:

```flux
import "timezone"
option location = timezone.location(name: "Asia/Kolkata")
```

This tells InfluxDB to split days at **IST midnight (18:30 UTC)** instead of UTC midnight, so each day's bucket correctly covers 00:00–23:59 IST.

On the backend, we pass the timezone name dynamically from the client system so the query always reflects the user's local timezone, not a hardcoded one.

---

**The Result**

After both fixes:
- Daily buckets now align to IST midnight, so data is bucketed into the correct IST day.
- Timestamps returned from the database reflect the start of each IST day.
- Graphs populate from IST 00:00, as expected.

---

**Key Takeaways**

- InfluxDB defaults to UTC for all windowing operations — always account for this when serving users in non-UTC timezones.
- The `timeSrc` parameter in `window()` controls whether bucket labels use start or end timestamps. Default is `_stop`; switch to `"_start"` to avoid off-by-one-day labeling.
- Use `import "timezone"` with `timezone.location()` to align InfluxDB's day boundaries with your users' local timezone.
- Pass timezone dynamically from the client rather than hardcoding it, so the solution scales across regions.

---
Give feedback on my LinkedIn or Twitter if any.

Thanks for reading. 