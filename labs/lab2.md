# Lab 2 · Reflection

**Name:Bolun Qiu**
**Due:** Sunday, November 8 at 11:59 PM

Answer in a few sentences each. Specific beats long.

**1. Your join.** Which column connected your two tables? How many rows went in, how many came out, and what did you do about anything that didn't match?
I joined EMS zipcode to ACS zcta. The inner join reduced 141,460 EMS rows to 139,845, excluding 1,615 unmatched records: 1,487 with missing ZIP codes and 128 with codes absent from the ACS table. I identified these with a left join and excluded them from the income comparison without deleting the original records.

**2. The reading.** Kleppmann describes tables (relational), documents (nested, like JSON) and graphs as different ways to model data. Would any of your two tables have been easier to work with as a document or a graph? Why or why not?
Neither dataset would be easier to work with as a document or graph: both have a clear tabular structure, with one row per EMS incident or ZCTA. Relational tables let many incidents share one income estimate without duplicating it; nested documents or graph connections would add little value to this comparison.

---

**Checklist before you submit**

- [ ] `notebooks/lab2_joins.ipynb`: Part A run in class, Part B completed with your own tables
- [ ] Every Part B text cell filled in (grain, unmatched rows, what the result tells you)
- [ ] This file answered
- [ ] Notebook saved (Cmd+S), then committed and pushed
- [ ] Repo URL submitted on CourseWorks
