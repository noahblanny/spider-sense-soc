# Alert investigation workflow

This is a **repeatable analyst checklist**, not a claim that every step is automated or currently implemented.

## 1. Establish the alert
- Record detection time, source system, rule name/ID, severity, and affected host.
- Preserve original event fields and the query or dashboard view used.
- Identify whether the event is endpoint telemetry, IDS data, or a correlation.

## 2. Validate context
- Check host identity, operating system, agent health, and collection gaps.
- Identify the relevant process, user, port, IP, protocol, or file path when available.
- Compare related events before and after the alert; note time zones.

## 3. Investigate
- Form competing hypotheses: expected administrative activity, benign application behavior, suspicious activity, or insufficient evidence.
- Use only authorized and appropriately scoped commands.
- Document commands, timestamps, outputs, and limitations; sanitize any evidence before publishing.

## 4. Decide and communicate
- Classify as **confirmed malicious**, **benign/expected**, or **inconclusive**, supported by the evidence.
- Separate observations from interpretation and avoid claiming attribution based on correlation alone.
- Describe impact, gaps, next steps, and detection improvements.

## 5. Improve detection
- Identify missing telemetry (for example, historical process-to-port mapping).
- Define a testable rule or data-collection change.
- Re-test with controlled lab activity and document results before calling a detection validated.

See [Investigation 001](../investigations/001-listening-port-changes.md) for a completed example.
