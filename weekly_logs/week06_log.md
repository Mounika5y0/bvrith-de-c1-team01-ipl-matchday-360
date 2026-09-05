# Week 06 Log — [Data Quality and Trust Layer
]

**Week:** 6  
**Date range:** [05-08-2026]  
**Team:** [IPL MatchDay 360
-team-01]  
**Project:** [IPL MatchDay 360
]

---

## 1. Sprint Goal

Implement Data Quality validation on the Silver layer to identify
trusted and invalid records.

The goal was to apply the agreed DQ rules, separate valid records
into Trusted Silver tables and invalid records into Quarantine tables,
and produce a clear DQ validation summary.


---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Define Data Quality rules | Team | Done | DQ documentation |
| Validate Silver match records | Team | Done | DQ notebook |
| Validate Silver delivery records | Team | Done | DQ notebook |
| Validate Silver player records | Team | Done | DQ notebook |
| Validate Silver venue records | Team | Done | DQ notebook |
| Create Trusted Silver tables | Team | Done | Databricks tables |
| Create Quarantine tables | Team | Done | Databricks tables |
| Generate DQ summary | Team | Done | DQ summary output |
| Validate trusted and quarantined record counts | Team | Done | Databricks validation |


---

## 3. Key Decisions

- Data Quality validation was performed after Silver transformation.
- Records passing the defined DQ rules were written to Trusted Silver.
- Records failing DQ rules were moved to Quarantine instead of being
  silently removed.
- Quarantine data was retained for traceability and investigation.
- Gold-layer processing was restricted to Trusted Silver data only.
- Delivery data was not promoted to Trusted Silver because it failed
  the applicable DQ validation.
---

## 4. Blockers / Risks

| **Dataset** | **Candidate Records** | **Trusted Records** | **Quarantine Records** | **Status** |
|---|---:|---:|---:|---|
| Matches | 500 | 476 | 24 | PASS |
| Deliveries | 119,879 | 0 | 119,879 | PASS |
| Players | 250 | 250 | 0 | PASS |
| Venues | 16 | 16 | 0 | PASS |

The DQ process completed successfully for all four datasets.

The delivery result is intentional: all candidate delivery records were
quarantined based on the defined Data Quality rules and therefore were
not promoted to Trusted Silver.

---

## 5. Evidence Added to GitHub

- [- Data Quality validation notebook
- DQ rules documentation
- Trusted Silver table outputs
- Quarantine table outputs]
- [<img width="747" height="475" alt="Screenshot 2026-09-05 141710" src="https://github.com/user-attachments/assets/4251e89f-f955-44c6-af7c-b80c53b65dba" />
<img width="758" height="462" alt="Screenshot 2026-09-05 141910" src="https://github.com/user-attachments/assets/32d26bef-89f6-42b4-b9af-c17d473ddd44" />
<img width="746" height="342" alt="Screenshot 2026-09-05 142344" src="https://github.com/user-attachments/assets/04c3e1f7-e68d-4c7f-8b86-fd4f0843f24f" />
]
- [`weekly_logs/week06_log.md`]

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI helped structure the Data Quality rules, validation queries, trusted/quarantine logic, and summary checks. |
| What we changed after AI suggestion | The suggested rules and code were adapted to the actual Silver schemas and Databricks environment. |
| What we verified manually | Candidate, trusted, and quarantine counts were checked in Databricks, along with the final DQ summary and table outputs. |
| What we can explain without AI | We can explain why each DQ rule is required, how records are classified as trusted or quarantined, and why quarantined records must not flow into Gold. |

---

## 7. Next Week Preparation

- Build the Gold dimensional and fact tables from Trusted Silver.
- Define Gold table grains and keys.
- Implement KPI calculations with explicit numerator,
  denominator, exclusions, and zero-denominator handling.
- Reconcile Gold outputs back to Trusted Silver.
