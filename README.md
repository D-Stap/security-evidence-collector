
[![Live Resume](https://img.shields.io/badge/Live%20Resume-grey?style=flat&labelColor=red&logo=readthedocs)](https://dafantestapletonresume.link)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-grey?style=flat&labelColor=0A66C2&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dafante-stapleton/)
[![GitHub](https://img.shields.io/badge/Profile-grey?style=flat&labelColor=181717&logo=github&logoColor=white)](https://github.com/D-Stap)
[![Credly](https://img.shields.io/badge/Credly-grey?style=flat&labelColor=FF6B00&logo=credly&logoColor=white)](https://www.credly.com/users/dafante-stapleton)
[![Email](https://img.shields.io/badge/Email-grey?style=flat&labelColor=EA4335&logo=gmail&logoColor=white)](mailto:dafante.e.stapleton.com)


# security-evidence-collector
A Python CLI that turns security scan results into **consistent, audit-friendly evidence**.


## Table of Contents
- [Purpose](#Purpose)
- [Scope](#scope)
- [Architecture](#architecture)
- [Installation](#installation)
- [CLI Usage](#cli-usage)
- [Outputs](#outputs)

## Purpose

**Security Evidence & Drift Collector** — A python CLI that:
- Runs standardized security scans against a repository (IaC + code) using **Trivy, Checkov, and Semgrep**
- Normalizes results into a consistent evidence schema (`evidence.json`)
- Evaluates outcomes against a policy baseline (`baseline.yml`)
- Produces audit-friendly artifacts (`summary.md`, `drift.json`) suitable for CI/CD and compliance workflows

## Scope 
### In scope (v1)
- Local repo scanning using Trivy / Checkov / Semgrep
- If a tool isn’t installed, it records that as “skipped” instead of crashing.
- Output normalization into a consistent schema
- Baseline-driven drift evaluation
- Deterministic exit codes for CI usage
- Artifact layout suitable for audit evidence retention

### Out of scope (v1)
- Full CSPM / cloud inventory
- Agent-based scanning
- SIEM ingestion pipeline
- Multi-cloud posture management

## Architecture
High-level flow:

1) Orchestrate scans (tool execution)
2) Capture raw tool output (`reports/scans/`)
3) Normalize to a unified schema (`reports/evidence.json`)
4) Evaluate baseline rules (`baseline.yml`) → `reports/drift.json`
5) Write an executive summary (`reports/summary.md`)
6) Return an exit code suitable for CI enforcement

Components:
- **CLI layer** (args, validation, exit codes)
- **Scanner wrappers** (run tools, capture raw JSON/SARIF)
- **Normalizer** (tool outputs → common finding schema)
- **Drift engine** (baseline rules → PASS/FAIL)
- **Report writer** (JSON + Markdown artifacts)

## Usage 
**Commands:**
```bash
sec-evidence collect --scan-path . --out reports/
sec-evidence scan --scan-path . --out reports/
sec-evidence drift --baseline baseline.yml --evidence reports/evidence.json
  
