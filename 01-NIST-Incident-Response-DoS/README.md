# Incident Report Analysis: DoS Attack Response using NIST CSF

## 1. Executive Summary
An organization providing multimedia, web design, and digital marketing services experienced a Denial of Service (DoS) attack that disrupted internal network availability for approximately two hours. The investigation revealed that an attacker exploited an unconfigured firewall to flood the internal network with ICMP packets (ping requests). The incident response team successfully mitigated the attack, restored network functionality, and implemented defensive hardening measures aligned with the NIST Cybersecurity Framework (CSF).

---

## 2. NIST Cybersecurity Framework Alignment

### Identify
* *Attack Type:* Denial of Service (DoS) via ICMP packet flood.
* *Impacted Assets:* Internal network bandwidth, shared network resources, and service delivery workflows.
* *Root Cause / Vulnerability:* An unconfigured perimeter firewall lacking packet filtering and rate-limiting rules.

### Protect
* *Firewall Hardening:* Implemented a new firewall rule enforcing rate limits on incoming ICMP traffic.
* *Anti-Spoofing:* Enabled source IP address verification to validate traffic origins and drop spoofed packets.
* *Policy Review:* Established regular security audits for firewall and network perimeter devices.

### Detect
* *Traffic Monitoring:* Deployed network monitoring software to establish traffic baselines and identify anomalous spikes.
* *Intrusion Detection/Prevention:* Configured an IDS/IPS system to inspect traffic patterns and alert on suspicious ICMP characteristics.

### Respond
* *Containment:* Blocked all incoming ICMP packets at the perimeter.
* *Service Management:* Temporarily took non-critical network services offline to preserve bandwidth for core operations.
* *Coordination:* Collaborated across IT and response teams to execute remediation workflows.

### Recover
* *Service Restoration:* Sequentially brought business-critical network services back online and confirmed stability.
* *Verification:* Monitored traffic flow to ensure normal network operational baseline.
* *Post-Incident Review:* Documented key lessons learned and updated response playbooks.

---
