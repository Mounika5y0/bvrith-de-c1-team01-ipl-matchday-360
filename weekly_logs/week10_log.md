# Week 10 Log — [ Controlled Streaming, Watermark, Deduplication and Drift]

**Week:** 10  
**Date range:** [Add dates]  
**Team:** [IPL_MatchDay_360 / Team-01]  
**Project:** [IPL MatchDay 360]

---

## 1. Sprint Goal

Implement and validate the controlled live-ball streaming pipeline using the prepared streaming event drops. The work covers explicit schema handling, streaming Bronze ingestion, data-quality validation, trusted/quarantine routing, watermark validation, event deduplication and live summary outputs.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Created four live-ball streaming JSON drops | Team | Done | `data_sample/streaming/live_ball_event_drop_001.json` to `_004.json` |
| Defined 20-field live event schema | Team | Done | `notebooks/07_streaming_simulation.ipynb` |
| Implemented streaming Bronze ingestion | Team | Done | `notebooks/07_streaming_simulation.ipynb` |
| Configured persistent streaming checkpoints | Team | Done | Databricks Volume checkpoint paths |
| Applied 10-minute `event_ts` watermark validation | Team | Done | Week 10 validation stream |
| Validated `event_id` deduplication | Team | Done | Duplicate check returned no duplicates |
| Checked `source_delivery_id` duplicates | Team | Done | Duplicate check |
| Applied arithmetic data-quality validation | Team | Done | `RUN_TOTAL_MISMATCH` validation |
| Routed invalid records to quarantine | Team | Done | `quarantine_live_ball_event` |
| Published trusted Silver events | Team | Done | `silver_live_ball_event` |
| Published live Gold summary | Team | Done | `gold_live_health_summary` |
| Verified no-new-record rerun behavior | Team | Done | Before = 12, After = 12, New records = 0 |

---


## 3. Key Decisions

- Used the Databricks Volume under `ipl_matchday_360.default.ipl_matchday_data` for streaming input and persistent checkpoints.
- Used an explicit 20-field event schema and retained `_rescued_data` for raw/rescued payload evidence.
- Used a 10-minute watermark on `event_ts` for streaming validation.
- Used `event_id` as the event-level deduplication key.
- Routed arithmetic-invalid events to the quarantine layer instead of the trusted Silver layer.
- Used persistent checkpoint locations because temporary streaming checkpoints were not supported by the current workspace/cluster.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| All four prepared streaming drops were available during the initial streaming run | The current result proves the end-to-end pipeline, but does not fully demonstrate four separate one-file-at-a-time progress states | Retain the prepared drops and document the controlled validation state |
| Temporary streaming checkpoints were unsupported | Streaming validation required persistent checkpoint locations | No further help required after using persistent checkpoints |

---

## 5. Evidence Added to GitHub

- `notebooks/07_streaming_simulation.ipynb`
- `data_sample/streaming/live_ball_event_drop_001.json`
- `data_sample/streaming/live_ball_event_drop_002.json`
- `data_sample/streaming/live_ball_event_drop_003.json`
- `data_sample/streaming/live_ball_event_drop_004.json`
- `streaming/kafka_event_schema.json`
- `streaming/structured_streaming_design.md`
- `docs/data_quality_summary.md`
- `weekly_logs/week10_log.md`


## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI helped with streaming pipeline troubleshooting, validation queries, checkpoint configuration and organizing the Week 10 implementation steps. |
| What we changed after AI suggestion | Persistent checkpoint locations were used after temporary streaming checkpoints were not supported. Validation queries were adjusted for the available cluster configuration. |
| What we verified manually | Bronze, Silver and Quarantine record counts, the quarantined arithmetic-invalid event, duplicate `event_id` results, schema fields and the zero-new-record rerun result were manually verified in Databricks. |
| What we can explain without AI | The team can explain the Bronze → DQ → Silver/Quarantine → Gold flow, event schema, checkpoint purpose, watermark concept, event deduplication and validation results. |

---


## 7. Next Week Preparation

- Review and finalize the Week 10 streaming evidence.
- Commit the completed Week 10 artifacts to GitHub.
- Continue with the next sprint requirements after Week 10 validation and documentation are finalized.
