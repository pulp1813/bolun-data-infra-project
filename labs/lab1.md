# Lab 1

**Name:Bolun Qiu**
**Due:** Sunday, October 25 at 11:59 PM
**Submit:** your repo URL in the Lab 1 assignment on CourseWorks

| Part | Where | File |
|---|---|---|
| **In class:** first DuckDB query on the penguins data | Session 1 | `notebooks/lab1_first_query.ipynb` |
| **At home:** two related tables of your own in DuckDB | Before Sunday | `notebooks/lab1_home_your_data.ipynb` |
| **At home:** lifecycle map of your data | Before Sunday | This file (below) |

---

## Lifecycle map of your data

In class we named five stages every data system has. Trace them for the two tables you loaded at home. Some rows ask what you did this week; the last two ask you to look ahead to the system you'll build by December. Educated guesses are fine; say where you're unsure.

**My tables:** EMS Incident Dispatch Data, October 2023 (`ems`) + ACS 2023 5-Year Median Household Income by ZCTA (`acs`)

| Stage | For your data | Your answer |
|---|---|---|
| **1. Ingestion** | Where did each file come from, and how did you get it (portal download, export, API)? If the source changed its format tomorrow, what would break? | The EMS data came from NYC Open Data through its API, filtered to October 2023 and saved as CSV. The income data came from the Census Bureau's 2023 ACS five-year B19013 bulk file. ZCTAs with codes appearing in the EMS data were selected and exported to CSV. Changes to column names, date formats, or geographic identifiers could break the extraction, loading, or matching steps.|
| **2. Validation** | What did `DESCRIBE`, `SUMMARIZE` and the key check reveal? Name one check you'd want to run *every time* new data arrives. | DESCRIBE showed that timestamps and numeric fields loaded with appropriate types, although geographic codes loaded as integers. SUMMARIZE and further checks identified special negative codes in the income data. After cleaning, 23 of 206 income estimates and 26 margins of error were missing. Of 225 distinct non-null EMS ZIP codes, 206 matched the ACS table and 19 did not. Each time new data arrives, I would check the percentage of EMS records with a non-null ZIP code that matches an ACS ZCTA code.|
| **3. Transformation** | What did you change to make the data usable (e.g. the missing-value fix)? What would you still need to clean, join or calculate to answer your question? | I converted the Census codes -666666666, -222222222 and -333333333 to NULL when loading the ACS table, without removing rows. Next, I would store geographic codes as five-character strings, investigate unmatched codes, and address the differences between ZIP codes and ZCTAs before joining. I would also filter valid response-time records, convert seconds to minutes, and calculate response-time summaries by area to compare with income.|
| **4. Scheduling** | How often does the source publish updates (check its website)? If your app used this data, how often would it need to refresh to stay useful, and why? | NYC Open Data lists the EMS dataset's update frequency as annual. The Census Bureau releases new ACS five-year estimates annually. For a historical comparison app, I would check for new releases monthly and refresh the data when a new annual release becomes available. The current CSVs are fixed extracts and will not update automatically.|
| **5. Interface** | Who would want answers from this data? Write the plain-English question they'd ask, and what a good answer would show them. | Urban planners, EMS service planners, and residents might ask: "Do lower-income areas in New York City experience longer EMS response times?" A useful answer would show a map and comparison chart of area-level income and response times, together with incident counts, missing-data rates, and income margins of error. It should explain that the EMS sample covers one month, income estimates cover 2019–2023, and an association would not establish causation.|

**Which stage feels most fragile for your data, and why?** (two or three sentences)
Transformation feels most fragile because EMS ZIP codes and Census ZCTAs are different geographic units, so matching their codes does not guarantee that their boundaries align. In addition, 19 of the 225 non-null EMS ZIP codes have no match, and 23 of the 206 ACS income estimates are missing; excluding these areas could bias the comparison.

---

## Checklist before you submit

- [x] In-class notebook: every cell ran, including your own query and three observations
- [x] At-home notebook: both CSVs in `data/`, both tables loaded and checked, the key relationship described, five queries each with a one-line note, three observations, two dataset cards
- [x] This lifecycle map filled in, plus the "most fragile" answer
- [x] Every notebook saved (Cmd+S), then committed and pushed
- [ ] Repo URL pasted into CourseWorks
