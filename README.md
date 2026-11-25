# BMAS NVL72 Command Plane

**Bare-Metal HPC & AI Orchestration — Engineered Without the Bloat.**

> **Interactive Architecture Diagram:** [Launch Live L0–L3 Animated Architecture (1440p)](https://proxy80.github.io/bmas-nvl72/)

The **BMAS NVL72 Command Plane** is a unified, voice-activated bare-metal management frontend engineered for **NVIDIA GB200 NVL72 racks** and large-scale HPC clusters. It delivers deterministic job scheduling, direct exporter telemetry (zero Grafana overhead), enterprise zero-trust security, and an embedded, air-gapped AI co-pilot across every infrastructure layer.

---

## Architecture Overview (L0 – L3)

* **L0 | Telemetry & Bare-Metal Inventory:** Pulls live cluster health, GPU metrics, and full-staff hardware inventory directly from native scrapers and exporters—no Grafana dependency required.
* **L1 | Scheduler & Cluster Balancing:** Native **Slurm 24.11+ REST API (`slurmrestd`)** integration for deterministic job orchestration and balanced resource allocation across bare-metal nodes.
* **L2 | Zero-Trust Security & Enterprise RBAC:** Built-in **mTLS / TLS / HTTPS** installer with automated certificate import, multi-role **RBAC**, **Active Directory** compatibility, and VPN-ready deployment.
* **L3 | Embedded AI with Auto-Follow & Voice Activation:** Context-aware AI embedded across **L0–L3** with **Auto-Follow**. Connects directly to your existing **on-prem OpenAI-compatible LLM API** (`vLLM`, `Ollama`, `TGI` via `BMAS_OX_MINI_URL`—no forced local LLM install on the appliance) and pairs with local, air-gapped **Piper ONNX** neural voice activation.

---

## Scaling & High Availability

* **Rapid Lab / Air-Gapped Bring-Up:** Ships with a local **SQLite** backend for instant deployment and testing.
* **Enterprise High Availability (10,000+ Nodes):** Scales seamlessly when backed by an external **MariaDB Galera Cluster**.

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

1. Download `bmas-nvl72-command-plane-1.0.0-rc1-linux-x86_64.tar.gz` from the **[Releases](https://github.com/proxy80/bmas-nvl72/releases)** tab.
2. Extract the release tarball and prepare a **Python 3.12** virtual environment (on RHEL/Rocky/Oracle Linux 9, install `python3.12` first via `sudo dnf install -y python3.12`):

    ```bash
    tar -xzf bmas-nvl72-command-plane-1.0.0-rc1-linux-x86_64.tar.gz
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

## Licensing & Support

* Includes a **30-day evaluation trial**.
* For enterprise licensing, bare-metal HPC consulting, or C2C/B2B deployment engagements, visit **[Bare Metal Applied Science LLC](https://baremetalappliedscience.com/)**.

© 2026 Bare Metal Applied Science LLC. All rights reserved.
