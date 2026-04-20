# System Security Plan (SSP)

**Project:** Self-Hosted Cybersecurity Infrastructure  
**Author:** Matthew Kilgore  
**Created:** 2026-04-20  
**Last Updated:** 2026-04-20  
**Version:** 1.0

---

## 1. System Overview

### 1.1 Purpose
This system provides a segmented self-hosted infrastructure environment used to support core services, storage, monitoring, security tooling, and AI/production workloads. This SSP exists to document the system architecture, security controls, current implementation state, and planned improvements as the environment matures.

### 1.2 System Name and Identifier
| Field | Value |
|-------|-------|
| System name | Self-Hosted Cybersecurity Infrastructure |
| System identifier | selfhost-secinfra-01 |
| System type | Personal project / self-hosted infrastructure environment |
| Environment | Physical + virtual |

### 1.3 System Description
The system is a self-hosted infrastructure environment built on Proxmox virtualization, UniFi networking, NAS-backed storage, and segmented VLANs for administrative, service, storage, monitoring, AI/production, and low-trust client traffic. Core services currently run on a Proxmox host in the services network, with storage segmented onto a dedicated NAS VLAN and validated cross-VLAN SMB access for dependent services. The environment is being expanded to support additional monitoring, IDS/IPS, vulnerability management, and AI/security-focused workloads in a production-style architecture.

---

## 2. System Categorization

### 2.1 Information Types
| Information type | Confidentiality | Integrity | Availability |
|-----------------|----------------|-----------|--------------|
| Configuration data | Low | Moderate | Moderate |
| Service data | Low | Moderate | Moderate |
| Monitoring and log data | Low | Moderate | Moderate |
| Administrative credentials | Moderate | High | Moderate |
| AI project artifacts / models | Low | Moderate | Moderate |

### 2.2 Overall System Impact Level
**Impact level:** Low

This is a personal self-hosted environment and not a production enterprise system handling regulated data. Integrity and availability still matter for service continuity, configuration stability, and realistic security operations practice, but overall system impact remains low.

---

## 3. System Environment

### 3.1 Network Topology
See `./diagrams/network-topology.png`.

Current logical segmentation:
- VLAN 10 — Admin
- VLAN 20 — Services
- VLAN 30 — AI / Production
- VLAN 40 — Storage
- VLAN 50 — Utility / Monitoring
- VLAN 60 — IoT / Media

### 3.2 Hardware Components
| Component | Type | Role | Location |
|-----------|------|------|----------|
| UniFi gateway/router | Physical | Network gateway / inter-VLAN routing | On-prem |
| UniFi 8-port switch | Physical | Switching / VLAN transport | On-prem |
| UniFi wireless AP | Physical | Wireless access / VLAN-mapped SSIDs | On-prem |
| Mini1 | Physical | Proxmox host for core services | On-prem |
| Ryzen AI server | Physical | Proxmox host for AI/production workloads | On-prem |
| UGREEN NAS | Physical | Centralized storage / SMB share hosting | On-prem |
| Raspberry Pi (DNS) | Physical | DNS / utility services | On-prem |
| Raspberry Pi (Monitoring) | Physical | Monitoring / logging / automation | On-prem |
| JetKVM | Physical | Remote administration / recovery | On-prem |
| Fanless firewall appliance | Physical | Planned dedicated firewall boundary for AI/production | On-prem |

### 3.3 Software Components
| Component | Version | Role |
|-----------|---------|------|
| Proxmox VE | Current deployed version | Virtualization platform |
| Ubuntu Server / Ubuntu Desktop VMs | Current deployed versions | Guest operating systems for services |
| Jellyfin | Current deployed version | Media service |
| Minecraft Java Server | Current deployed version | Game service |
| SMB/CIFS client | Current deployed version | Cross-VLAN NAS mount access |
| UniFi Network | Current deployed version | Network management |
| Wazuh | Planned / partial deployment | SIEM / monitoring |
| Suricata | Planned / partial deployment | IDS/IPS |
| Docker | Planned / partial deployment | Containerized tooling and services |
| OPNsense/pfSense | Planned | Dedicated firewall for AI/production boundary |

### 3.4 Network Boundaries
In scope:
- Admin VLAN 10
- Services VLAN 20
- AI/Production VLAN 30
- Storage VLAN 40
- Utility/Monitoring VLAN 50
- IoT/Media VLAN 60
- Virtualized workloads, storage dependencies, and internal management paths

Out of scope:
- External cloud services not directly hosted in this environment
- Third-party SaaS platforms
- Public internet services beyond standard upstream access

---

## 4. Security Controls

> Mapped to NIST SP 800-53. Marked as **Implemented**, **Planned**, or **N/A**.

### 4.1 Access Control (AC)
| Control | Status | Description |
|---------|--------|-------------|
| AC-2 — Account management | Implemented | Local account management is in place for infrastructure hosts and service systems. Administrative accounts are restricted to intended management users. |
| AC-3 — Access enforcement | Implemented | Access is being enforced through VLAN segmentation, service placement, and host/service-level authentication. Additional firewall tightening is planned. |
| AC-17 — Remote access | Planned | Administrative remote access exists in limited form; JetKVM and tighter admin-only access paths are planned for improved control and recovery. |

### 4.2 Audit and Accountability (AU)
| Control | Status | Description |
|---------|--------|-------------|
| AU-2 — Event logging | Planned | Basic system and service logs exist. Centralized logging and security event collection with Wazuh are planned. |
| AU-9 — Protection of audit info | Planned | Log protection is partially provided through host controls and NAS-backed storage, but dedicated log retention and protection controls are still being built out. |

### 4.3 Configuration Management (CM)
| Control | Status | Description |
|---------|--------|-------------|
| CM-2 — Baseline configuration | Implemented | Baseline network design, VLAN layout, host placement, and service dependencies have been documented. |
| CM-6 — Configuration settings | Implemented | Static IP changes, mount configuration updates, service config validation, and VLAN mappings are being explicitly managed and documented. |
| CM-7 — Least functionality | Planned | Segmentation is in place, but inter-VLAN policy tightening and service exposure reduction are still in progress. |

### 4.4 Identification and Authentication (IA)
| Control | Status | Description |
|---------|--------|-------------|
| IA-2 — User identification | Implemented | Administrative access requires valid local or service credentials on managed systems. |
| IA-5 — Authenticator management | Planned | Credential handling exists for service mounts and local admin access, but rotation and centralization are not yet formalized. |

### 4.5 System and Communications Protection (SC)
| Control | Status | Description |
|---------|--------|-------------|
| SC-7 — Boundary protection | Implemented | VLAN segmentation is in place for management, services, storage, AI/production, monitoring, and low-trust clients. Dedicated AI firewall boundary is planned. |
| SC-8 — Transmission confidentiality | Planned | Internal service traffic protections vary by service. Additional review of admin, storage, and monitoring paths is planned. |
| SC-28 — Protection at rest | Planned | Storage exists on NAS-backed infrastructure. Encryption at rest and formalized storage protection controls are not yet fully documented. |

### 4.6 System and Information Integrity (SI)
| Control | Status | Description |
|---------|--------|-------------|
| SI-2 — Flaw remediation | Implemented | Ubuntu, Jellyfin, and Minecraft server components were recently updated as part of migration and maintenance activities. Ongoing patching is part of the operating model. |
| SI-3 — Malicious code protection | Planned | IDS/IPS and SIEM capabilities using Suricata and Wazuh are planned as security monitoring and detection layers. |

---

## 5. Planned Configurations (Linked to POA&M)

See [POAM.md](./POAM.md) for the full plan of action and milestones tied to the controls above.

Current high-priority planned items include:
- Migrate AI Proxmox host to VLAN 30
- Validate AI workload functionality after migration
- Move utility systems to VLAN 50
- Restrict administrative access to VLAN 10 only
- Tighten inter-VLAN firewall policy
- Place AI/production behind a dedicated firewall boundary
- Redesign Proxmox host management to live on Admin VLAN while guests remain on service-specific VLANs

---

## 6. Incident Response

### 6.1 Process
Security or service issues are identified through direct service failures, connectivity problems, application testing, system logs, and planned monitoring capability. Initial response consists of identifying the affected host, service, or network segment; validating recent changes; isolating the issue to configuration, routing, service, or dependency failure; and restoring baseline functionality. Changes are documented in the project tracker and supporting notes, with longer-term hardening or cleanup actions captured in the POA&M.

### 6.2 Backup and Recovery
| Data / System | Backup method | Frequency | Recovery target |
|---------------|--------------|-----------|----------------|
| Proxmox host configuration | Manual config backup / documented settings | On major change | < 4 hours |
| Service VM configs | In-guest config files / documented rebuild steps | On major change | < 4 hours |
| Jellyfin media mount configuration | `/etc/fstab` and credential file review | On change | < 1 hour |
| NAS-hosted service data | NAS-backed storage | Ongoing / persistent | Depends on service |
| Minecraft server jar rollback | Previous jar retained as `.old` | On update | < 1 hour |
| Architecture and network documentation | Markdown / GitHub repo / local docs | On change | < 1 hour |

---

## 7. Document Control

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-04-20 | Initial draft based on current segmented infrastructure design, completed service migrations, and planned next-phase hardening |