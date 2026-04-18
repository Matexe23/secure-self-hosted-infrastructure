# Self-Hosted Infrastructure and AI Platform

Segmented self-hosted infrastructure built with Proxmox, UniFi networking, NAS-backed storage, and dedicated AI/production services.

## Overview

This repository documents the design, deployment, and ongoing development of a self-hosted infrastructure platform built to support core services, centralized storage, segmented networking, and dedicated AI/production workloads.

The environment is designed with a focus on operational reliability, service isolation, practical security controls, and long-term support for custom development and AI-focused projects.

## Objectives

- Build and maintain a segmented self-hosted infrastructure environment
- Separate management, service, storage, utility, and AI/production traffic by function
- Support centralized NAS-backed storage for persistent data, backups, and project artifacts
- Improve operational maturity through documentation, monitoring, and recovery planning
- Provide a stable platform for AI experimentation, automation, and future engineering projects

## Core Infrastructure

### Compute
- Mini PC running Proxmox for core self-hosted services
- Ryzen AI mini PC with 128 GB RAM running Proxmox for AI/production workloads
- 2 Raspberry Pi 5 systems for utility, monitoring, and supporting services

### Storage
- UGREEN NAS providing centralized storage for application data, backups, and project artifacts

### Networking
- UniFi router, switch, and wireless access point
- Segmented VLAN design for management, services, storage, utility systems, and low-trust devices
- Planned dedicated firewall boundary for AI/production workloads

### Management
- JetKVM for remote administration and recovery
- Proxmox management separated from service traffic
- Administrative access restricted to a dedicated management segment

## Network Segmentation

The environment is organized around functional separation between management, services, storage, monitoring, and higher-value workloads.

- **VLAN 10 — Management**  
  Administrative access for infrastructure management, Proxmox, network gear, JetKVM, and storage management

- **VLAN 20 — Core Services**  
  Self-hosted applications and stable day-to-day services

- **VLAN 30 — AI / Production**  
  Dedicated zone for AI workloads, custom development, and higher-value production-style services

- **VLAN 40 — Storage**  
  NAS-backed storage for service data, backups, models, and project artifacts

- **VLAN 50 — Utility / Monitoring**  
  Monitoring, logging, DNS, and supporting automation services

- **VLAN 60 — IoT / Media**  
  Lower-trust client devices and media consumers with restricted internal access

## Platform Design Principles

- Separate control plane and service plane access
- Reduce unnecessary east-west traffic between systems
- Restrict administrative access to dedicated management paths
- Limit storage access by host and purpose
- Treat AI/production workloads as protected assets
- Build around repeatability, observability, and maintainability

## Current Services and Capabilities

The platform currently supports self-hosted services, centralized storage, and dedicated infrastructure for AI/production expansion. Existing and planned capabilities include:

- Self-hosted media and application services
- NAS-backed persistent storage
- Proxmox-based virtualization
- Remote administration and recovery workflows
- Network segmentation by trust and function
- Utility services for monitoring, logging, and automation
- Dedicated compute for AI-focused workloads and custom projects

## Current Focus

- Finalizing VLAN segmentation and service placement
- Separating hypervisor management from workload traffic
- Protecting the AI/production environment behind a dedicated policy boundary
- Tightening access between services, storage, and management networks
- Expanding documentation and operational visibility

## Planned Work

- Architecture and network diagrams
- Firewall rule intent documentation
- Service inventory and dependency mapping
- Monitoring and logging improvements
- Backup validation and recovery workflows
- AI project integration and experiment support
- Additional automation for health checks and operational validation

## Why This Project Exists

This platform is intended to serve as both a reliable self-hosted infrastructure environment and a foundation for more advanced engineering work. Rather than functioning as a simple home network, it is being developed as a structured infrastructure platform that emphasizes segmentation, operational discipline, and support for future AI and systems projects.

## Roadmap

### Near Term
- Complete VLAN implementation
- Validate service placement and connectivity
- Document management, service, and storage boundaries
- Stand up utility and monitoring functions

### Mid Term
- Add dedicated firewall protection for AI/production workloads
- Improve centralized logging and visibility
- Validate backup and recovery workflows
- Expand automation for system health and operations

### Long Term
- Integrate custom AI projects into the platform
- Support repeatable experimentation and artifact storage
- Continue improving security boundaries and operational maturity

## Status

This project is actively being built and documented. The current priority is establishing clean network segmentation, protected workload boundaries, and a strong foundation for future AI and infrastructure work.
