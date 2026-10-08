# Investigation 001 — Listening Port Changes

## Alert Summary

Wazuh detected changes in the listening ports on a monitored
MacBook. The alert indicated that TCP and UDP ports had opened
or closed between monitoring intervals.

The activity was investigated to determine whether the changes
were caused by legitimate applications or potentially
unauthorized services.

## Evidence Collected

### Wazuh Port Monitoring Logs

Wazuh recorded changes in the MacBook's listening TCP and UDP
ports between monitoring intervals.

### Log Comparison

The `diff -u` command was used to compare the previous and
current port snapshots.

This allowed me to identify which ports were added or removed
without manually comparing the entire logs.

Notable observations:
- UDP port 50289 appeared and later disappeared.
- TCP port 54609 was removed.
- TCP port 54910 was added.

### Process Investigation

The `lsof` command was used to identify processes associated
with currently listening network ports.

The `ps` command was then used to inspect a specific process,
including its PID, user, and executable command.

This helped verify that the IPNExtension process belonged
to the intentionally installed Tailscale application.

However, some ports had already closed by the time they were
investigated, preventing historical process attribution.

## Findings and Analysis

The investigation identified multiple changes in TCP and UDP
listening ports on the monitored MacBook.

Comparing consecutive Wazuh logs revealed that some ports
 appeared temporarily and disappeared in later snapshots.

Live process inspection confirmed that Tailscale was running
and using UDP port 41641. However, this port was not one of
the changes responsible for the investigated alert.

Attempts to identify the processes associated with past
ports 50289 (UDP) and 54910 (TCP) were unsuccessful because
those ports were no longer active when inspected.

No evidence of malicious activity was established, but the
available telemetry was insufficient to attribute the
historical port changes to specific processes.

**Classification:** Inconclusive

## Recommendations

One improvement I would make to Spider-Sense is recording which processes are using network ports and keeping a history of that activity.

The monitoring system should collect:
- Process name and PID
- Executable path
- Port number and protocol (TCP/UDP)
- Listening address
- Timestamps showing when activity occurred
- Historical process and port activity

Having this information would make investigations easier because we could identify which applications were using specific ports, even after those ports had closed.

## Conclusion

**Classification:** Inconclusive

During this investigation, I found no evidence confirming malicious activity. However, I couldn't determine which applications were responsible for some of the port changes because those ports were no longer active when I checked.

This investigation helped me identify a limitation in my monitoring setup. Adding historical process and network activity would give Spider-Sense better visibility and make future investigations more effective.
