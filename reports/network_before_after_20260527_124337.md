# Network Before/After Portfolio Report

Generated: 20260527_124337

- Before CSV: logs\network_summary_20260527_121144.csv
- After CSV: logs\network_summary_20260527_124337.csv

## Executive Summary

- Overall result: **MIXED**.
- Matched scenarios: 1.
- Average display FPS delta: -0.210 fps.
- Average p95 latency delta: -0.034 ms.
- Total drop delta: 0 frames (0.0% change).
- Deadline drop delta: 0, output queue drop delta: 0.
- Deadline NACK recovered-frame delta: 0 frames.
- Deadline NACK expired-drop delta: 0 frames.

## Portfolio Snapshot

| Item | Summary |
| --- | --- |
| What changed | Compared persisted `network_summary_*.csv` runs and generated a scenario-matched evaluation report. |
| Why it matters | The engine now demonstrates the full implementation -> measurement -> evaluation -> improvement loop expected in realtime networking work. |
| Evidence source | CSV telemetry condensed into scenario summaries, then compared by scenario name and adaptive mode. |
| Primary result | MIXED across 1 matched scenarios. |
| Main trade-off | Low-latency recovery may intentionally expire stale frames instead of displaying late video. |

## Key Metrics

| Metric | Before | After | Delta | Interpretation |
| --- | ---: | ---: | ---: | --- |
| Avg display FPS | 27.288 | 27.078 | -0.210 | roughly unchanged |
| Avg p95 latency ms | 0.331 | 0.297 | -0.034 | roughly unchanged |
| Total frame drops | 1 | 1 | 0 | unchanged |
| Deadline drops | 1 | 1 | 0 | display-deadline pressure |
| Output queue drops | 0 | 0 | 0 | renderer/jitter backlog pressure |
| NACK recovered frames | 0 | 0 | 0 | selective retransmit recovery evidence |
| NACK expired drops | 0 | 0 | 0 | stale recovery discarded before missing deadline |

## Scenario Comparison

| Scenario | Mode Before | Mode After | Cause Before | Cause After | FPS Before | FPS After | FPS Delta | P95 Before ms | P95 After ms | P95 Delta ms | Drops Before | Drops After | Drop Delta | Drop Change | NACK Recovered Before | NACK Recovered After | NACK Expired Before | NACK Expired After |
| --- | --- | --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Baseline / Fixed Quality | Fixed Quality | Fixed Quality | None | None | 27.288 | 27.078 | -0.210 | 0.331 | 0.297 | -0.034 | 1 | 1 | 0 | 0.0% | 0 | 0 | 0 | 0 |

## After-Run Mode Winners

| Network Scenario | Best Mode | Why It Wins | FPS | P95 Latency ms | Drops | NACK Recovered | Trade-off |
| --- | --- | --- | ---: | ---: | ---: | ---: | --- |
| Baseline | Fixed Quality | kept interactive FPS under the latency deadline | 27.078 | 0.297 | 1 | 0 | none observed |

## Automatic Evaluation

- Baseline / Fixed Quality: no material metric change.

## Interview-Ready Summary

### What Changed

The engine compares two persisted experiment runs directly from `network_summary_*.csv`, matches scenarios by name, and reports FPS, p95 latency, frame drops, deadline drops, output queue drops, NACK recovery, and stale-recovery expiry.

### Why It Matters

Realtime video networking is not only about sending packets. This report shows whether reliability and adaptive-control changes actually improved the user-visible pipeline under repeatable network conditions.

### Evidence

- Overall result: MIXED.
- Average display FPS delta: -0.210 fps.
- Average p95 latency delta: -0.034 ms.
- Total frame drop delta: 0 frames.
- NACK recovered-frame delta: 0 frames.

### Trade-Off

The low-latency policy may drop or expire incomplete frames that cannot arrive before the display deadline. That is intentional for interactive video: a late frame is less useful than a fresh frame.

### Next Improvement

The next portfolio-level step is to add time-series charts for FPS, p95 latency, QoE score, target quality, and recovery events so the report shows both summary evidence and how the controller reacted over time.
