![CI](https://github.com/YOUR_GITHUB_ORG/YOUR_REPO/actions/workflows/ci.yml/badge.svg)
![Nightly Scan](https://github.com/YOUR_GITHUB_ORG/YOUR_REPO/actions/workflows/nightly.yml/badge.svg)
![Last Commit](https://img.shields.io/github/last-commit/YOUR_GITHUB_ORG/YOUR_REPO)
![Issues](https://img.shields.io/github/issues/YOUR_GITHUB_ORG/YOUR_REPO)
![Python](https://img.shields.io/badge/python-3.11%2B-blue)
![Code Style: Ruff](https://img.shields.io/badge/code%20style-ruff-000000)

# security-evidence-collector
A Python CLI that turns security scan results into **consistent, audit-friendly evidence**.


## Table of Contents
- [Purpose](#Purpose)
- [Scope](#scope)
- [Architecture](#architecture)
- [Installation](#installation)
- [CLI Usage](#cli-usage)
- [Outputs](#outputs)
- [Baseline Policy](#baseline-policy)
- [Evidence Schema](#evidence-schema)
- [CI/CD Integration](#cicd-integration)
- [Security & Data Handling](#security--data-handling)

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
  
