readme_content = '''# Music Streaming User Engagement & Revenue Analytics
### SQL + Excel | Real Dataset (Last.fm 1K Users)

## Overview
End-to-end analytics project analyzing real music-streaming user behavior: engagement (DAU/MAU),
retention (cohort analysis), churn, top content, and revenue — built with SQLite for analysis and
Excel for the reporting layer.

## Dataset
**Source:** Last.fm Dataset - 1K Users (Celma, 2010, Universitat Pompeu Fabra) — a real, published
research dataset distributed with Last.fm's permission for non-commercial use.
Retrieved from: https://github.com/eifuentes/lastfm-dataset-1K

- **~992 real users**, real signup dates, real country/demographics
- **~19.15 million real listening events**, timestamped, Feb 2005 - May 2009
- No song-level data loss: every play, artist, and track name is genuine

**What's simulated, and why:** no public dataset combines song-level listening events with
subscription-plan/billing data — that combination is commercially sensitive and never released
publicly. `plan_type` and `monthly_price` are assigned per real user based on their real listening
volume (heavier real listeners are modeled as more likely to be paying subscribers). Every other
field — every play, every date, every user's real profile — is unmodified real data.

## Data Quality Notes
- **Analysis cutoff: 2009-05-31.** Two stray rows dated 2010 and 2013 are scrape/timestamp errors
  outside the true collection window and were excluded.
- **May 2009 is a partial month** (data collection ends mid-month) — this drags down that month's
  DAU/MAU stickiness and the tail of the revenue trend; treat it as incomplete, not a real decline.
- **Early cohorts (2004) predate the first tracked listen (Feb 2005)** — some users' Last.fm accounts
  existed before scrobble tracking began for them. Expected, not an error.
- **Late cohorts (2007+) have very small sample sizes** (1-2 users each), so their retention swings
  to 0% or 100% with no middle ground — not statistically meaningful, shown for completeness only.
- **"Antarctica" appears as a user country** — a small number of Last.fm users self-report novelty
  locations in their profile; low sample size, noted rather than removed.
- **No `genre` field exists in this dataset** — "Top Songs by Genre" was replaced with "Top Songs by
  Country" using the same ranking logic.

## Methodology by Day

**Day 1 — Setup & EDA:** Loaded the 2.5GB events file via chunked pandas processing (1M rows/chunk)
directly into SQLite to keep memory flat; loaded and cleaned user profiles; verified row counts,
date ranges, and null rates.

**Day 2 — DAU/MAU & Cohort Retention:** Computed daily/monthly active users and a stickiness ratio
(avg DAU / MAU). Built cohort retention by grouping users by real signup month and tracking what
% of each cohort was still listening N months later.

**Day 3 — Churn, Top Songs, Revenue:** Defined churn as 60+ days of inactivity relative to the
dataset's last active date. Ranked top tracks overall and by country. Modeled monthly revenue as
each user's simulated plan price charged for every real month they were actually active.

**Day 4 — Excel Dashboard:** Pivot-ready raw data table (plan x country x month) for hands-on
PivotTable practice, a formula-driven pivot summary, a conditional-formatting retention heatmap,
churn/top-songs bar charts, and a revenue forecast using SLOPE/INTERCEPT linear regression — with
a second, more conservative forecast fit only on the most recent 12 months, since the full-history
trend overstates future revenue (real growth clearly decelerates from 2008 onward).

## Key Findings
- Stickiness (DAU/MAU) held steady in the mid-50s to low-60s% range for most of the dataset's life —
  a healthy sign of habitual, not occasional, usage.
- Free-tier churn (25.6%) was roughly 3x higher than Premium (8.6%) — though this is partly circular
  since plan was assigned from listening volume; a real product would need actual subscription data
  to separate "churns because free" from "was already a light user."
- Real top tracks (Such Great Heights, Karma Police, Love Will Tear Us Apart) reflect genuine
  mid-2000s indie/alternative listening patterns.
- Revenue growth was steep through 2006-2007 and clearly decelerating by 2008-2009 — a full-history
  linear forecast overstates where revenue is headed; a recent-trend forecast is more defensible.

## Tools
SQLite (SQL), Python/pandas (chunked ETL), openpyxl (Excel automation: pivot tables, conditional
formatting, formulas, charts)
'''

with open("README.md", "w", encoding="utf-8") as f:
    f.write(readme_content)

print("README.md saved to your project folder.")