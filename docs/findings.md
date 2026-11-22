# Findings report

## Finding 1

**What I found:** All 200 sampled incidents occurred on October 1, 2023, and all five NYC boroughs were represented.

**From table:** `course.main.ems_borough_day`

**The query or row that shows it:**
```sql
SELECT incident_day,
       SUM(sample_incidents) AS incidents,
       COUNT(DISTINCT borough) AS boroughs
FROM course.main.ems_borough_day
GROUP BY incident_day
ORDER BY incident_day;
```

**A question a user could ask the app to discover this:** For each date in the sample, how many incidents were recorded and how many boroughs were represented?

## Finding 2

**What I found:** Of the 200 sampled incidents, 188 (94%) had a valid response time. The remaining 12 were excluded when calculating median response times.

**From table:** `course.main.ems_borough_summary`

**The query or row that shows it:**
```sql
SELECT SUM(sample_incidents) AS total_incidents,
       SUM(valid_incidents) AS valid_incidents,
       SUM(sample_incidents - valid_incidents) AS excluded_incidents,
       ROUND(100.0 * SUM(valid_incidents) / SUM(sample_incidents), 1) AS valid_pct
FROM course.main.ems_borough_summary;
```

**A question a user could ask the app to discover this:** How many incidents had a valid response time, and what percentage of the sample did they represent?

## Finding 3

**What I found:** Among incidents with valid response times, Brooklyn had the highest median (12.57 minutes, 37 incidents) and Staten Island had the lowest (6.93 minutes, 11 incidents). The gap between the reported medians was 5.64 minutes.

**From table:** `course.main.ems_borough_summary`

**The query or row that shows it:**
```sql
SELECT borough, valid_incidents, median_response_min
FROM course.main.ems_borough_summary
ORDER BY median_response_min DESC;
```

**A question a user could ask the app to discover this:** Which boroughs had the highest and lowest median response times in this sample, and how many incidents contributed to each median?

## What this data can't tell you

The sample contains only 200 incidents from one day. It cannot establish whether the 5.64-minute gap persists over time or whether it is related to neighborhood income.
