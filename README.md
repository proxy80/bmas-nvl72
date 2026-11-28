# BMAS NVL72 Command Plane

**Bare-Metal HPC & AI Orchestration — Engineered Without the Bloat.**

> **Interactive Architecture Diagrams (1440p Live Simulators):**
> - **L0–L3 Telemetry & Co-Pilot Stack:** [Launch Live L0–L3 Animated Architecture](https://proxy80.github.io/bmas-nvl72/)
> - **Database Capacity, Galera Failover & 180-Day Query (ACME Rocket Company Case Study):** [Launch Interactive Capacity & Failover Simulator](https://proxy80.github.io/bmas-nvl72/acme-capacity-architecture.html)

The **BMAS NVL72 Command Plane** is a unified, voice-activated bare-metal management frontend engineered for **NVIDIA GB200 NVL72 racks** and large-scale HPC clusters. It delivers deterministic job scheduling, direct exporter telemetry (zero Grafana overhead), enterprise zero-trust security, and an embedded, air-gapped AI co-pilot across every infrastructure layer.

---

## Architecture Overview (L0 – L3)

* **L0 | Telemetry & Bare-Metal Inventory:** Pulls live cluster health, GPU metrics, and full-staff hardware inventory directly from native scrapers and exporters—no Grafana dependency required.
* **L1 | Scheduler & Cluster Balancing:** Native **Slurm 24.11+ REST API (`slurmrestd`)** integration for deterministic job orchestration and balanced resource allocation across bare-metal nodes.
* **L2 | Zero-Trust Security & Enterprise RBAC:** Built-in **mTLS / TLS / HTTPS** installer with automated certificate import, multi-role **RBAC**, **Active Directory** compatibility, and VPN-ready deployment.
* **L3 | Embedded AI with Auto-Follow & Voice Activation:** Context-aware AI embedded across **L0–L3** with **Auto-Follow**. Connects directly to your existing **on-prem OpenAI-compatible LLM API** (`vLLM`, `Ollama`, `TGI` via `BMAS_OX_MINI_URL`—no forced local LLM install on the appliance) and pairs with local, air-gapped **Piper ONNX** neural voice activation.

---

## Scaling & High Availability

> **Live Interactive Walkthrough:** [Launch the ACME Rocket Company Database Capacity, Galera Failover & 180-Day Query Simulator (1440p)](https://proxy80.github.io/bmas-nvl72/acme-capacity-architecture.html)

This 6-stage interactive architecture simulator demonstrates how a single compiled binary scales from a single-row lab to a 150-rack AI factory across **GB200 NVL72**, **GB300 NVL72**, and **H100 / B200 Pods**:

- **Edge Standalone (1–8 Racks — Local SQLite):** Zero-config bring-up using built-in **SQLite** with automatic 180-day retention purge and no external DBA required.
- **Enterprise HA Cutover (10–150+ Racks — 3-Node MariaDB Galera):** Shows why single-writer SQLite hits lock contention (`SQLITE_BUSY`) at 46+ racks (828+ OOB Redfish BMCs + Slurm REST), and how exporting `BMAS_DB_URL` transitions cleanly to a synchronous **3-Node MariaDB Galera Cluster** with automatic schema creation.
- **Zero Split-Brain 3-Bus Separation:** Decouples 180-day relational job history (Galera), 2-second live GPU telemetry (Prometheus/DCGM TSDB—zero SQL overhead), and the `core/db_sync.py` hardware safety net (`TRAY_COLS` mirrored to local SQLite).
- **Interactive Galera Failover & 180-Day Query Drill:** Lets you simulate both a **single-node failure** (2/3 quorum, zero downtime) and a **full 0/3 Galera outage** (live 2s Prometheus + local SQLite BMC fallback + delta resync on recovery), plus fire live **180-day `slurm_job_history` queries** with Ox Mini AI summaries.

---

## Recommended OX Mini Co-Pilot Models

**OX Mini** is model-agnostic and connects to any OpenAI-compatible `/v1/chat/completions` endpoint via `BMAS_OX_MINI_URL`, `BMAS_OX_MINI_MODEL`, and optional `BMAS_OX_MINI_API_KEY` (Bearer token) (no local LLM weights are bundled or forced during install).

| Deployment Tier | Recommended Models | Notes |
| :--- | :--- | :--- |
| **Lab / Single GPU (8–24 GB VRAM)** | `qwen2.5-coder:latest` (tested baseline), `Qwen3.8-27B`, `Gemma 4 26B A4B` | Fast local Ollama/vLLM baseline for Slurm, PromQL, and Redfish diagnostics. |
| **Enterprise On-Prem (vLLM / TGI)** | `Qwen3-Coder-Next`, `Llama 4 Scout`, `DeepSeek-V4-Flash` | High-concurrency, long-context inference on dedicated on-prem GPU nodes. |
| **Cloud / Hosted API (Non-Air-Gapped)** | Google Gemini, OpenAI, or enterprise OpenAI-compatible gateway | Drop-in option for non-air-gapped deployments. |

---

## Minimum Requirements

* **OS:** Enterprise Linux (Rocky Linux 9 / RHEL 9 / Oracle Linux 9) or Ubuntu 22.04 / 24.04 LTS
* **Scheduler:** Slurm **24.11+** with `slurmrestd` enabled
* **Telemetry:** Native node/GPU exporters (Prometheus-compatible scrapers)
* **Security:** TLS/mTLS certificates (auto-generated or imported during install)

---

## Quick Start (30-Day Evaluation Trial)

1. Download `bmas-nvl72-command-plane-1.0.0-rc3-linux-x86_64.tar.gz` from the **[Releases](https://github.com/proxy80/bmas-nvl72/releases)** tab.
2. Extract the release tarball and prepare a **Python 3.12** virtual environment (on RHEL/Rocky/Oracle Linux 9: `sudo dnf install -y python3.12`; on Ubuntu 24.04 LTS: `sudo apt update && sudo apt install -y python3.12-venv`):

    ```bash
    tar -xzf bmas-nvl72-command-plane-1.0.0-rc3-linux-x86_64.tar.gz
    cd bmas_nvl72_release_stage
    python3.12 -m venv venv && source venv/bin/activate
    pip install --upgrade pip
    pip install -r requirements.txt
    ```

3. Run the zero-mutation pre-flight check to verify runtime dependencies, port `8720`, TLS PKI target permissions, and compiled `.so` binary integrity before modifying the host:

    ```bash
    ./install.sh --dry-run
    ```

4. Initialize local TLS PKI, the append-only audit database, your admin passphrase, and an optional on-prem LLM API endpoint (`BMAS_OX_MINI_URL`), then launch the Command Plane:

    ```bash
    ./install.sh
    ./bmas-nvl72.sh
    ```

5. Sign in to the Command Plane HUD to connect your Slurm 24.11+ REST endpoints, Prometheus/DCGM exporters, Redfish BMCs, and optional MariaDB Galera HA backend (`BMAS_DB_URL`).

---

## Licensing & Enterprise Pricing

BMAS NVL72 Command Plane is licensed **flat-rate per cluster**—with **zero per-GPU, per-core, or per-node metering** and **100% offline Ed25519 cryptographic activation**. There is no phone-home telemetry, no cloud dependency, and no surprise scaling tax as you expand your hardware footprint.

| Tier | Licensing | Annual Renewal | Deployment Scope |
| :--- | :--- | :--- | :--- |
| **EVAL** | **$0** (30 Days) | — | Full L0–L3 evaluation for lab and pre-production validation |
| **ENTERPRISE** | **$34,999.99** (Year 1) | **$5,000 / year** | Production NVL72 & HPC clusters (1–8 racks, SQLite or Galera HA, offline Ed25519 key) |
| **SOVEREIGN** | **$75,000.00** *(Custom)* | Custom SLA | Multi-rack scale-out (10–150+ racks, 3-Node MariaDB Galera HA, dedicated B2B onboarding) |

### Why Flat-Rate Sovereign Licensing
- **Predictable CapEx & OpEx:** One flat license covers your entire control plane—whether you are orchestrating a single rack or scaling out across high-density liquid-cooled nodes.
- **Zero Metering:** No per-GPU fees, no CPU core counting, and no seat-based restrictions.
- **Air-Gap Native:** Offline Ed25519 license keys validate locally in microseconds with zero external network calls.
- **Complete L0–L3 Stack Included:** Native Redfish/Prometheus telemetry, Slurm 24.11+ REST orchestration, mTLS/RBAC security, 180-day job auditing, and the embedded AI Co-Pilot are included out of the box.

**Enterprise Procurement:** B2B Purchase Order, Wire, or ACH via **Bare Metal Applied Science LLC**.

For enterprise licensing, bare-metal HPC consulting, or C2C/B2B deployment engagements, visit [Bare Metal Applied Science LLC](https://baremetalappliedscience.com/).

© 2026 Bare Metal Applied Science LLC. All rights reserved.
