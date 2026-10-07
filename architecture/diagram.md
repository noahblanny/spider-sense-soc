# Spider-Sense SOC Architecture Diagram

```mermaid
flowchart TB

    NET["Home Network"]

    MAC["MacBook<br/>macOS<br/>Wazuh Agent"]
    WEB["webcrawler<br/>Linux Laptop<br/>Wazuh Agent"]
    HOME["homeweb<br/>Physical Server<br/>Proxmox VE"]

    KALI["Kali Linux VM<br/>Wazuh Agent"]

    subgraph PROXMOX["Proxmox Environment"]
        WAZUH["SpiderSense VM<br/>Wazuh Server"]
        SURICATA["Suricata Sensor VM<br/>Suricata IDS<br/>Wazuh Agent<br/>ens18 → vmbr0"]
    end

    NET --- MAC
    NET --- WEB
    NET --- HOME

    MAC --> KALI
    HOME --> PROXMOX

    MAC -. "Endpoint Telemetry" .-> WAZUH
    KALI -. "Endpoint Telemetry" .-> WAZUH
    WEB -. "Endpoint Telemetry" .-> WAZUH
    SURICATA -. "Security Events" .-> WAZUH
```

## Data Flow

Wazuh agents installed across the environment forward endpoint security telemetry to the centralized SpiderSense Wazuh server.

The dedicated Suricata sensor analyzes network traffic visible to its interface and forwards security telemetry to Wazuh through its Wazuh agent.

> **Current limitation:** The Suricata sensor is connected to the standard Proxmox `vmbr0` bridge and does not currently have guaranteed visibility into all home network traffic.
