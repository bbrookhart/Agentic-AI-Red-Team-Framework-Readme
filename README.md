# 🛡️ Agentic AI Red Team Framework

> A comprehensive, ethical security testing framework for Agentic AI systems — aligned with NIST AI RMF, OWASP LLM Top 10, MITRE ATLAS, and ISO/IEC 42001.

---

## ⚠️ Legal & Ethical Notice

This framework is designed **exclusively for authorized security testing** of systems you own or have explicit written permission to test. Unauthorized use is illegal and unethical. All tests should be conducted in isolated, non-production environments. The authors assume no liability for misuse.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Test Modules](#test-modules)
- [Compliance Mapping](#compliance-mapping)
- [Configuration](#configuration)
- [Reports](#reports)
- [Contributing](#contributing)

---

## Overview

The **Agentic AI Red Team Framework** (AART) provides structured, repeatable, and auditable security assessments for AI agent systems including:

- LLM-based autonomous agents
- Multi-agent orchestration pipelines
- RAG (Retrieval-Augmented Generation) systems
- Tool-using AI systems (function calling, code execution)
- AI systems with memory / persistent state

### Key Principles

| Principle | Implementation |
|-----------|---------------|
| **Safety First** | All tests run in sandboxed environments with kill switches |
| **Auditability** | Full logging of every test action and result |
| **Compliance** | Mapped to NIST AI RMF, OWASP LLM Top 10, MITRE ATLAS |
| **Repeatability** | Deterministic test cases with seeded randomness |
| **Defense-Oriented** | Every finding includes remediation guidance |

---

## Architecture

```
agentic-ai-redteam/
├── src/
│   ├── agents/           # Red team agent implementations
│   │   ├── orchestrator.py       # Main test orchestrator
│   │   ├── prompt_injector.py    # Prompt injection agent
│   │   ├── adversarial_agent.py  # Adversarial input generator
│   │   └── exfil_agent.py        # Data exfiltration tester
│   ├── modules/          # Test modules by attack category
│   │   ├── prompt_injection.py   # OWASP LLM01
│   │   ├── insecure_output.py    # OWASP LLM02
│   │   ├── training_data.py      # OWASP LLM03
│   │   ├── model_dos.py          # OWASP LLM04
│   │   ├── supply_chain.py       # OWASP LLM05
│   │   ├── sensitive_info.py     # OWASP LLM06
│   │   ├── plugin_security.py    # OWASP LLM07
│   │   ├── excessive_agency.py   # OWASP LLM08
│   │   ├── overreliance.py       # OWASP LLM09
│   │   └── model_theft.py        # OWASP LLM10
│   ├── reporters/        # Report generators
│   │   ├── html_reporter.py
│   │   ├── json_reporter.py
│   │   └── compliance_mapper.py
│   └── utils/            # Utilities
│       ├── logger.py
│       ├── config_loader.py
│       ├── rate_limiter.py
│       └── sandbox.py
├── configs/
│   ├── default.yaml      # Default test configuration
│   ├── strict.yaml       # Strict/comprehensive mode
│   └── targets/          # Target-specific configs
├── reports/              # Generated reports (gitignored)
├── tests/                # Unit tests for the framework itself
├── docs/                 # Additional documentation
└── .vscode/              # VS Code workspace settings
```

---

## Installation

### Prerequisites

- Python 3.11+
- Git
- VS Code with recommended extensions (see `.vscode/extensions.json`)

### Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_ORG/agentic-ai-redteam.git
cd agentic-ai-redteam

# Create virtual environment
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure your target
cp configs/default.yaml configs/targets/my-target.yaml
# Edit my-target.yaml with your target details

# Run pre-flight checks
python -m src.utils.preflight
```

---

## Quick Start

```bash
# Run full red team suite against a target
python -m src.agents.orchestrator --config configs/targets/my-target.yaml --output reports/

# Run a specific module only
python -m src.modules.prompt_injection --target http://localhost:8000 --verbose

# Generate compliance report from existing results
python -m src.reporters.compliance_mapper --input reports/results.json --framework nist
```

---

## Test Modules

| Module | OWASP LLM | MITRE ATLAS | Severity |
|--------|-----------|-------------|----------|
| Prompt Injection | LLM01 | AML.T0051 | Critical |
| Insecure Output Handling | LLM02 | AML.T0048 | High |
| Training Data Poisoning | LLM03 | AML.T0020 | High |
| Model Denial of Service | LLM04 | AML.T0034 | Medium |
| Supply Chain Vulnerabilities | LLM05 | AML.T0010 | High |
| Sensitive Information Disclosure | LLM06 | AML.T0056 | Critical |
| Insecure Plugin Design | LLM07 | AML.T0043 | High |
| Excessive Agency | LLM08 | AML.T0054 | Critical |
| Overreliance | LLM09 | AML.T0050 | Medium |
| Model Theft | LLM10 | AML.T0044 | High |

---

## Compliance Mapping

| Standard | Coverage |
|----------|----------|
| NIST AI RMF 1.0 | GOVERN, MAP, MEASURE, MANAGE |
| OWASP LLM Top 10 2025 | All 10 categories |
| MITRE ATLAS | 15+ techniques |
| ISO/IEC 42001 | Annex A controls |
| NIST SP 800-53 | SI, CA, RA control families |

---

## Configuration

See `configs/default.yaml` for all available options with documentation.

---

## Reports

Reports are generated in `reports/` (gitignored by default). Each run produces:

- `results.json` — Raw findings in machine-readable format
- `report.html` — Human-readable report with severity ratings
- `compliance.json` — Compliance mapping for each finding
