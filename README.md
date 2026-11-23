# BMAS NVL72 Command Plane

**Bare-Metal HPC & AI Orchestration — Engineered Without the Bloat.**

> **Interactive Architecture Diagram:** [Launch Live L0–L3 Animated Architecture (1440p)](https://proxy80.github.io/bmas-nvl72/)

The **BMAS NVL72 Command Plane** is a unified, voice-activated bare-metal management frontend engineered for **NVIDIA GB200 NVL72 racks** and large-scale HPC clusters. It delivers deterministic job scheduling, direct exporter telemetry (zero Grafana overhead), enterprise zero-trust security, and an embedded, air-gapped AI co-pilot across every infrastructure layer.

---

## Architecture Overview (L0 – L3)

* **L0 | Telemetry & Bare-Metal Inventory:** Pulls live cluster health, GPU metrics, and full-staff hardware inventory directly from native scrapers and exporters—no Grafana dependency required.
* **L1 | Scheduler & Cluster Balancing:** Native **Slurm 24.11+ REST API (`slurmrestd`)** integration for deterministic job orchestration and balanced resource allocation across bare-metal nodes.
* **L2 | Zero-Trust Security & Enterprise RBAC:** Built-in **mTLS / TLS / HTTPS** installer with automated certificate import, multi-role **RBAC**, **Active Directory** compatibility, and VPN-ready deployment.
* **L3 | Embedded AI with Auto-Follow & Voice Activation:** Context-aware AI embedded across **L0–L3** with **Auto-Follow**. Understands live metrics, explains every layer as you navigate, and ships with local, air-gapped neural voice activation.

---

## Scaling & High Availability

* **Rapid Lab / Air-Gapped Bring-Up:** Ships with a local **SQLite** backend for instant deployment and testing.
* **Enterprise High Availability (10,000+ Nodes):** Scales seamlessly when backed by an external **MariaDB Galera Cluster**.

---

## Minimum Requirements

* **OS:** Enterprise Linux (Rocky Linux 9 / RHEL 9 / Oracle Linux 9) or Ubuntu 22.04 / 24.04 LTS
* **Scheduler:** Slurm **24.11+** with `slurmrestd` enabled
* **Telemetry:** Native node/GPU exporters (Prometheus-compatible scrapers)
* **Security:** TLS/mTLS certificates (auto-generated or imported during install)

---

## Quick Start (30-Day Evaluation Trial)

1. Download `bmas-nvl72-command-plane-1.0.0-rc1-linux-x86_64.tar.gz` from the **[Releases](https://github.com/proxy80/bmas-nvl72/releases)** tab.
2. Extract and run the installer in an isolated directory:

    tar -xzf bmas-nvl72-command-plane-1.0.0-rc1-linux-x86_64.tar.gz
    cd bmas-nvl72-command-plane-1.0.0-rc1-linux-x86_64
    sudo ./install.sh

3. Follow the interactive prompts to configure mTLS/HTTPS certificates, Slurm 24.11+ REST endpoints, and your SQLite or MariaDB Galera backend.

---

## Licensing & Support

* Includes a **30-day evaluation trial**.
* For enterprise licensing, bare-metal HPC consulting, or C2C/B2B deployment engagements, visit **[Bare Metal Applied Science LLC](https://baremetalappliedscience.com/)**.

© 2026 Bare Metal Applied Science LLC. All rights reserved.
