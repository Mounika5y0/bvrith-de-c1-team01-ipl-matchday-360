# Week 03 Log — [Third week]

**Week:** 3  
**Date range:** [25-07-2026 to 31-07-2026]  
**Team:** [IPL MatchDay 360 / Team 01]  
**Project:** [IPL MatchDay 360]

---

## 1. Sprint Goal

Profile the IPL MatchDay 360 datasets by loading the source files into Databricks, validating their schemas, checking data quality, and analyzing relationships between datasets before building the Bronze layer.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Loaded matches.csv into Databricks | Punna Mounika | ✅ Done | 01_Data_Profiling_SQL.ipynb |
| Loaded deliveries.parquet into Databricks | Punna Mounika | ✅ Done | 01_Data_Profiling_SQL.ipynb |
| Loaded players.json into Databricks | Punna Mounika | ✅ Done | 01_Data_Profiling_SQL.ipynb |
| Loaded venues.csv into Databricks | Punna Mounika | ✅ Done | 01_Data_Profiling_SQL.ipynb |
| Performed schema validation | Punna Mounika | ✅ Done | Notebook output |
| Calculated record counts | Punna Mounika | ✅ Done | Notebook output |
| Performed null value analysis | Punna Mounika | ✅ Done | Notebook output |
| Verified primary keys | Punna Mounika | ✅ Done | Notebook output |
| Performed relationship analysis | Punna Mounika | ✅ Done | Notebook output |
| Performed join risk analysis | Punna Mounika | ✅ Done | Notebook output |
| Exported notebook as Jupyter (.ipynb) | Punna Mounika | ✅ Done | 01_Data_Profiling_SQL.ipynb |

---

## 3. Key Decisions

- Used Databricks SQL with Unity Catalog to perform dataset profiling and validation.
- Completed data quality checks before proceeding to Bronze layer development.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| Column name differences between datasets (for example, `player_of_match_id` instead of `player_of_match`) | Some SQL queries failed initially | Verified the schema using `DESCRIBE` and updated the SQL queries accordingly |

---

## 5. Evidence Added to GitHub

- Updated `weekly_logs/week03_log.md`
- Added `01_Data_Profiling_SQL.ipynb`
- Added Week 3 notebook screenshots
- Updated project progress

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | Assisted in writing SQL queries for data loading, profiling, relationship analysis, and join validation. |
| What we changed after AI suggestion | Updated SQL queries to match the actual column names in the datasets after checking the schema. |
| What we verified manually | Verified query execution, schema details, record counts, null analysis, and relationships directly in Databricks. |
| What we can explain without AI | The dataset loading process, schema validation, profiling results, and relationship analysis performed during Week 3. |

---

## 7. Next Week Preparation

- Begin Bronze layer data ingestion.
- Implement metadata tracking and rerun behaviour for Bronze tables.
