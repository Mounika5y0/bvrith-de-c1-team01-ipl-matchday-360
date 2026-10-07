# Structured Streaming Design

**Week:** 10  
**Project:** IPL MatchDay 360  
**Purpose:** Controlled file streaming simulation using Databricks Auto Loader / Structured Streaming.

---

## 1. Streaming Scenario

Four JSON event drops simulate live IPL ball-by-ball events.

New JSON files are added one file at a time to the streaming input path. Databricks Auto Loader detects each new file, Structured Streaming processes the events using the explicit 20-field schema, and the processed records are written to the Streaming Bronze layer.

The pipeline also applies event-time watermarking, event_id deduplication, source delivery duplicate detection, and data-quality routing.

Flow:

> JSON event drop → Auto Loader → Explicit Schema → Bronze → Watermark + Deduplication → DQ Validation → Trusted Silver / Quarantine → Live Health Summary

Kafka is documented only as production architecture awareness. Kafka implementation is not required for this internship.

---

## 2. Event Source

| **Item** | **Description** |
| --- | --- |
| Event file format | JSON |
| Input path | `/Volumes/workspace/default/p01_ipl_matchday_360/streaming_input/` |
| Processing method | Databricks Auto Loader / Structured Streaming |
| Event schema | Explicit 20-field IPL live-ball event schema |
| Watermark | 10 minutes on `event_ts` |
| Deduplication key | `event_id` |
| Duplicate delivery flag | `source_delivery_id` |
| Bronze table | `bronze_live_ball_event` |
| Trusted Silver table | `silver_live_ball_event` |
| Quarantine table | `quarantine_live_ball_event` |
| Gold summary | `gold_dq_and_live_summary` |
| Checkpoint path | `/Volumes/workspace/default/p01_ipl_matchday_360/checkpoints/live_ball_event/` |

---

## 3. Event Schema

The live-ball event contract contains 20 required fields:

1. `event_id`
2. `event_ts`
3. `schema_version`
4. `match_id`
5. `innings_no`
6. `over_no`
7. `ball_seq`
8. `event_type`
9. `batting_team_id`
10. `bowling_team_id`
11. `batter_id`
12. `non_striker_id`
13. `bowler_id`
14. `runs_batter`
15. `runs_extras`
16. `total_runs`
17. `wicket_flag`
18. `dismissed_player_id`
19. `source_delivery_id`
20. `event_sequence`

Compatible data types are defined in `streaming/kafka_event_schema.json`.

---

## 4. Streaming State and Controls

### Watermark

A 10-minute event-time watermark is applied using `event_ts`.

Purpose:

- Handle reasonably late events.
- Bound streaming state.
- Make late-event behavior observable.

### Deduplication

`event_id` is used as the primary event deduplication key.

Repeated `event_id` values must not create duplicate trusted events.

Repeated non-null `source_delivery_id` values are separately flagged so delivery-level duplication can be monitored without replacing event identity.

---

## 5. Data Quality and Routing

The streaming pipeline validates:

- Event identity and required fields
- Event sequence
- Match / innings / over / ball references
- Run arithmetic
- Wicket consistency
- Compatible data types
- Malformed JSON
- Unexpected or drifted fields

Records that fail required-field, type, malformed-input, or error-level checks are retained in:

`quarantine_live_ball_event`

Unexpected extra fields are retained as rescued/drift evidence where supported.

Trusted records are written to:

`silver_live_ball_event`

---

## 6. Near-Real-Time Metrics

The live health summary tracks:

| **Metric** | **Purpose** |
| --- | --- |
| Input rows | Number of rows received |
| Trusted rows | Events accepted into the trusted layer |
| Quarantine rows | Events routed to quarantine |
| Duplicate events | Repeated `event_id` count |
| Duplicate deliveries | Repeated `source_delivery_id` count |
| Late events | Events affected by late-event handling |
| Latest event time | Most recent `event_ts` |
| Per-drop row count | Events processed from each file drop |

These metrics are used to monitor streaming health during the four incremental file drops.

---

## 7. Incremental Processing Test

The four event drops are processed one at a time:

1. Add Drop 001 and record streaming progress.
2. Add Drop 002 and verify only new data is processed.
3. Add Drop 003 and inspect malformed, late, drift, and quarantine behavior.
4. Add Drop 004 and record final streaming progress.

The checkpoint is retained throughout the test.

---

## 8. No-New-File Rerun

After Drop 004, the streaming process is run again without adding a new input file.

Expected result:

- New input rows = `0`
- Trusted row count remains unchanged
- Quarantine row count remains unchanged
- Existing checkpoint remains intact
- No duplicate processing of previously consumed files

The checkpoint must not be deleted or reset for this test.

---

## 9. Output Layers

| **Layer** | **Purpose** |
| --- | --- |
| Bronze | Raw streaming events with ingestion/file lineage |
| Silver | Trusted, validated and deduplicated events |
| Quarantine | Malformed, invalid or error-level events |
| Gold | Streaming health, DQ and reconciliation summary |

Bronze should retain useful lineage such as source filename, ingestion timestamp, drop identifier, and raw/rescued/malformed context where available.

---

## 10. Limitations

- This is a student streaming simulation, not a production event platform.
- Kafka is documented as production architecture awareness only.
- Streaming events are synthetic and educational.
- The file source simulates live event arrival.
- The checkpoint must be preserved during incremental and rerun testing.
