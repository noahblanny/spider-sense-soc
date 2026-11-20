# Detection engineering roadmap

A prioritized backlog for Spider-Sense. **Items here are proposals, not deployed detections.**

| Priority | Improvement | Why it matters | Validation approach |
| --- | --- | --- | --- |
| High | Historical process/port telemetry | Live `lsof` cannot identify a process after a transient port closes | Generate a temporary listener in an authorized lab and confirm timestamped process attribution survives closure |
| High | Sensor visibility assessment | `vmbr0` attachment does not imply network-wide capture | Compare known test traffic paths with Suricata observations and document blind spots |
| Medium | Agent health / ingestion monitoring | Stale or disconnected agents can create false confidence | Stop/restart a test agent and verify monitoring and recovery |
| Medium | Alert enrichment | Raw events can lack process, user, and asset context | Compare enriched vs. original triage records |
| Medium | Detection rule documentation | Repeatable testing improves reliability | Store sanitized rule, expected behavior, test data, and observed outcome |
| Low | Alert trend reporting | Helps prioritize analysis, not establish incident counts | Separate raw events, alerts, and confirmed findings in reports |

## Definition of done for a detection

1. Document the threat behavior and telemetry source.
2. Specify rule logic and scope.
3. Run an authorized positive test and a benign negative test.
4. Record observed output and limitations.
5. Publish sanitized evidence and a maintenance note.

## Existing investigation-driven gap

[Investigation 001](../investigations/001-listening-port-changes.md) could not attribute transient listening ports after they closed. Historical process/port collection is therefore the first recommended improvement.
