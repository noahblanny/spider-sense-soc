# Spider-Sense SOC Architecture

## Overview

Spider-Sense is a virtualized home SOC environment built around a centralized Wazuh server. Wazuh agents installed across physical and virtual endpoints send security telemetry to the SpiderSense VM for centralized monitoring and investigation.

Suricata operates on a dedicated Proxmox VM and provides network-based detection. The Suricata sensor also runs a Wazuh agent so Suricata-generated events can be forwarded to the Wazuh server.

## Architecture

                    Home Network
                         |
        +----------------+----------------+
        |                |                |
     MacBook         webcrawler         homeweb
    Physical Mac     Linux Laptop    Physical Server
        |                |                |
    Wazuh Agent      Wazuh Agent       Proxmox VE
        |                                 |
     Kali VM                    +---------+---------+
    Wazuh Agent                 |                   |
                          SpiderSense VM       Suricata VM
                           Wazuh Server         Suricata IDS
                                               Wazuh Agent
                                               ens18 -> vmbr0
                                                    |
                                                    |
                                      Suricata Security Events
                                                    |
                                                    v
                                              Wazuh Server

## Components

### homeweb
Physical server running Proxmox VE and hosting the virtualized components of the Spider-Sense environment.

### SpiderSense VM
Dedicated virtual machine running the central Wazuh server. It receives and analyzes security telemetry from Wazuh agents across the homelab.

### Suricata Sensor VM
Dedicated Proxmox virtual machine running Suricata IDS.

Suricata monitors traffic available to its `ens18` interface, which is connected to the Proxmox `vmbr0` bridge. A Wazuh agent on the VM forwards security telemetry and Suricata events to the SpiderSense Wazuh server.

### MacBook
Physical macOS endpoint monitored by a Wazuh agent.

### Kali VM
Kali Linux virtual machine hosted on the MacBook and independently monitored by a Wazuh agent.

### webcrawler
Physical Linux laptop monitored by a Wazuh agent.

## Current Wazuh Agents

- MacBook
- Kali
- webcrawler
- Suricata Sensor

## Data Flow

1. Wazuh agents collect endpoint security telemetry.
2. Agents forward telemetry to the SpiderSense Wazuh server.
3. Suricata analyzes network traffic visible to its sensor interface.
4. Suricata generates network security events.
5. The Wazuh agent on the Suricata VM forwards relevant telemetry to the Wazuh server.
6. Wazuh centralizes the data for alerting, analysis, dashboards, and investigation.

## Current Limitations

The Suricata sensor is connected to the standard Proxmox `vmbr0` bridge. This does not guarantee visibility into all traffic traversing the home network. Network-wide traffic mirroring or another dedicated monitoring architecture may be implemented in a future version of the lab.

## Planned Improvements

- Improve Suricata network visibility
- Document alert investigation workflows
- Map detections to MITRE ATT&CK
- Add security automation with Python
- Document detection rules and configuration
- Add sanitized architecture and dashboard screenshots
