# Data dictionary: NYC EMS incident response

## Source and refresh schedule

**Source:** [FDNY EMS Incident Dispatch Data](https://data.cityofnewyork.us/Public-Safety/EMS-Incident-Dispatch-Data/76xm-jjuj/about_data) · [API documentation](https://dev.socrata.com/foundry/data.cityofnewyork.us/76xm-jjuj)

**Refreshed:** GitHub Actions is scheduled to run daily at 11:17 UTC (6:17 a.m. EST / 7:17 a.m. EDT). FDNY updates the source annually. Each successful EMS run replaces all three tables using a sample of 200 incidents dated October 1, 2023 or later. If validation fails, the previous tables stay in place.

## Table 1: EMS incident sample

**Table name on MotherDuck:** `course.main.ems_nyc_sample`

**One row is:** One sampled EMS incident, identified by `incident_id`.

**Removed on purpose:** The API excludes incidents before October 1, 2023 and returns at most 200 records. Incidents with invalid response times remain in the table so they are still counted. The source label `RICHMOND / STATEN ISLAND` is standardized to `STATEN ISLAND`.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| `incident_id` | VARCHAR | Unique identifier for the incident. | Text ID | `78327772` |
| `incident_datetime` | TIMESTAMP | When the incident was created in the EMS dispatch system. | Date and time | `2023-10-01 00:00:17` |
| `borough` | VARCHAR | Borough where the incident occurred. | BRONX, BROOKLYN, MANHATTAN, QUEENS, STATEN ISLAND | `BRONX` |
| `zipcode` | VARCHAR | ZIP code of the incident location, when available. | Five-digit text code | `10467` |
| `dispatch_response_seconds_qy` | BIGINT | Time from incident creation to the first unit assignment. | Seconds | 20 |
| `incident_travel_tm_seconds_qy` | BIGINT | Time from the first unit assignment to the first arrival on scene. | Seconds | 438 |
| `incident_response_seconds_qy` | BIGINT | Total time from incident creation to the first arrival on scene. | Seconds | 458 |
| `valid_incident_rspns_time_indc` | VARCHAR | Whether FDNY considers the total response time valid. | Y = valid; N = invalid | `Y` |

## Table 2: Borough response summary

**Table name on MotherDuck:** `course.main.ems_borough_summary`

**One row is:** One borough across the entire sample. This table has five rows.

**Source:** Calculated from `course.main.ems_nyc_sample`.

**Removed on purpose:** All incidents count toward `sample_incidents`. Only incidents flagged `Y` count toward `valid_incidents` and the median response time.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| `borough` | VARCHAR | Borough represented by this row. | One of the five NYC boroughs | `BROOKLYN` |
| `sample_incidents` | BIGINT | All sampled incidents in the borough. | Incident count | 42 |
| `valid_incidents` | BIGINT | Sampled incidents with a valid total response time. | Incident count | 37 |
| `median_response_min` | DOUBLE | Median total response time among valid incidents, converted to minutes and rounded to two decimal places. | Minutes | 12.57 |

## Table 3: Daily incident counts by borough

**Table name on MotherDuck:** `course.main.ems_borough_day`

**One row is:** One borough on one calendar date represented in the sample.

**Source:** Calculated from `course.main.ems_nyc_sample` using the date portion of `incident_datetime`.

**Removed on purpose:** All sampled incidents are counted, including those with invalid response times. A borough and date with no sampled incidents has no row; this does not mean no EMS incidents occurred there.

### Columns

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| `incident_day` | DATE | Date the incident was created in the dispatch system. | YYYY-MM-DD | `2023-10-01` |
| `borough` | VARCHAR | Borough represented by this row. | One of the five NYC boroughs | `MANHATTAN` |
| `sample_incidents` | BIGINT | Number of sampled incidents in this borough on this date. | Incident count | 57 |

## What this data can't tell you

1. All 200 sampled incidents occurred on October 1, 2023. They do not represent every NYC incident that day and cannot establish a trend over time.
2. Only 188 of 200 incidents had valid total response times. To calculate a citywide sample median, use those 188 incident records; averaging the five borough medians does not give the citywide median.
3. These tables contain no neighborhood income, traffic, or call severity data. They can show where response times differed in the sample, but they cannot explain why.

## Example questions it can answer

1. How many sampled incidents had a valid total response time, and what percentage of the sample did they represent?
2. Which borough had the highest median total response time, and how many valid incidents contributed to that median?
3. How many sampled incidents occurred in each borough on October 1, 2023?