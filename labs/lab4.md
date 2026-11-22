# Labs 3 + 4 · Pipeline submission

**Name:Bolun Qiu**
**Due:** Sunday, November 22 at 11:59 PM

## Links

**A successful run of your pipeline** (Actions tab → the green run → copy the address):https://github.com/pulp1813/bolun-data-infra-project/actions/runs/45080545116

**A deliberately failed run** (Run workflow with "Break the data on purpose" ticked → the red run → copy the address):https://github.com/pulp1813/bolun-data-infra-project/actions/runs/45080864012

**Your table on MotherDuck:** `course.main.ems_nyc_sample`

## Reflections

Answer in a few sentences each. Specific beats long.

**1. Your checks.** Which of your checks would you be most worried to see fail, and what would go wrong for a user of your app if bad data got through?
I’m most concerned about the five-borough check. In my sample, all 11 Staten Island incidents were labeled RICHMOND / STATEN ISLAND; overlooking that label would leave an entire borough out of the response-time comparison.

**2. Run it twice.** What happens to your MotherDuck table if your pipeline runs twice in one day? How do you know?
Each successful run replaces course.main.ems_nyc_sample, so running it twice should leave 200 rows, not 400. I can verify this by checking that COUNT(*) and COUNT(DISTINCT incident_id) both return 200 after the second run.

**3. Your schedule.** How often does your source actually update? Is daily the right schedule for it, and would you take a full copy or only what's new?
NYC Open Data lists the EMS dataset as updated annually, so a daily refresh is unnecessary. I would reload my full 200-row sample after each release to capture corrections to older incidents.

**4. What leaves your control.** Next week an AI model will see your table's description and users' questions. Is there anything in your data, or anything a user might type, that shouldn't be sent to a free AI service? Why or why not?
The model only needs the table description to answer questions about borough-level patterns. I would keep patient names, exact addresses, and medical details out of prompts because they could identify someone who requested emergency care.

---

## Set up your secrets (once)

Your MotherDuck token is a password. It goes in two secret stores, **never** in a file, a notebook, or an AI chat.

**Get a token:** app.motherduck.com → your name (bottom left) → **Settings** → **Access Tokens** → **Create token**. Name it `codespaces`. Copy it.

**A. For your Codespace** (so `python pipeline.py` can publish):
github.com → your profile picture → **Settings** → **Codespaces** → **New secret**. Name `MOTHERDUCK_TOKEN`, paste the token, choose your course repo under **Repository access**, **Add secret**. Then restart your Codespace.

**B. For GitHub Actions** (so the daily run can publish):
your course repo on github.com → **Settings** (repo tab, not your profile) → **Secrets and variables** → **Actions** → **New repository secret**. Name `MOTHERDUCK_TOKEN`, paste the same token, **Add secret**.

If a token ever leaks, delete it in MotherDuck right away and make a new one (then update both secrets).

---

**Checklist before you submit**

- [x] `project.toml` points to **your** API, with at least five checks of your own (each with a why) and at least three tables: one clean table and at least two analysis tables (trend, key metrics or ranking)
- [x] `docs/findings.md`: three findings, each backed by one of your tables
- [x] A green run and a red run on the Actions tab, both linked above
- [x] `docs/data_dictionary.md` filled in for every table you publish
- [x] This file answered
- [x] No token anywhere in your files (tokens start with `eyJ`: search your repo with Cmd+Shift+F)
- [x] Committed and pushed; repo URL submitted on CourseWorks
