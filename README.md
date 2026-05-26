# Agentic AI Compliance Framework

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Beta        |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

This repository provides compliance mappings, deployment checklists, and gap
analyses for regulated organisations deploying AI agent orchestrators. It covers
Paperclip (primary reference implementation), n8n, custom Claude API agents, and
emerging orchestrators.

---

## Why this exists

AI agent orchestrators — Paperclip, n8n, LangGraph, CrewAI, custom Claude API
agents — have no built-in compliance frameworks. Their documentation does not
address the obligations that apply when you process personal data, operate in
regulated sectors, or need to demonstrate governance to auditors.

This repo fills that gap. It maps orchestrator capabilities against UK GDPR, EU
GDPR, the EU AI Act, ISO 42001, and NIST CSF. Paperclip is the primary reference
implementation; mappings note where controls are orchestrator-agnostic and where
they are Paperclip-specific. It documents what each tool does well, what it does
not do, and what you need to build or document yourself before you go live with
real client data.

This is not legal advice. It is not a certification. It is a practical starting
point, written by practitioners, for teams who need to move quickly without
cutting corners.

---

## Who this is for

Teams running, or evaluating, AI agent orchestrators in regulated UK or EU
environments. Specifically:

- Developers building agent-based products that will handle personal data
- DevOps teams responsible for an agentic AI deployment at a regulated
  organisation
- Compliance leads who need to understand what an agentic AI deployment means
  for their obligations

This repo assumes you have an orchestrator running locally and are now asking:
"What do we need before we can use this with real client data?"

Sector-specific notes are included for fintech (FCA), legal (SRA), and
healthcare (DSPT, ICO Children's Code).

---

## What this covers

| Framework | Scope                                                             |
| --------- | ----------------------------------------------------------------- |
| UK GDPR   | Article-by-article control mapping, gap register, worked examples |
| EU GDPR   | As UK GDPR; divergences noted where relevant                      |
| EU AI Act | Risk classification guidance for agentic AI use cases             |
| ISO 42001 | AI management system controls mapped to agentic deployments       |
| NIST CSF  | Cybersecurity framework controls for the hosting infrastructure   |

---

## How to use it

1. Read `frameworks/gdpr/control-mapping.md` to understand your baseline
   obligations and your orchestrator's current gaps.
2. Work through `runbooks/deployment-checklist.md` before go-live. Every
   checkbox is there for a reason.
3. Review `gap-analysis/agentic-gdpr-gaps.md` and decide which workarounds you
   will implement.
4. Pick the worked example closest to your scenario and use it as a template.
5. If you are in a regulated sector, check the relevant sector directory under
   `frameworks/`.

Start with the checklist. It will surface the documents and decisions you need.

---

## Repo structure

```text
agentic-ai-compliance/
  frameworks/
    gdpr/
      control-mapping.md          # Article-by-article UK GDPR mapping
      worked-example-health-authority-processor.md
    eu-ai-act/                    # Risk classification and obligations
    iso-42001/                    # AI management system controls
    nist-csf/                     # Cybersecurity framework controls
  runbooks/
    deployment-checklist.md       # Pre-go-live compliance checklist
    incident-response.md          # Breach detection and notification runbook
    erasure-runbook.md            # Right to erasure workflow
  gap-analysis/
    agentic-gdpr-gaps.md          # Full gap register with workarounds
  templates/
    dpa-template.md               # Data Processing Agreement template
    ropa-template.md              # Records of Processing Activities template
    dpia-template.md              # Data Protection Impact Assessment template
    privacy-notice-template.md    # Article 13/14 privacy notice template
```

Not all directories contain files yet. The gap register tracks what is planned.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Maintained by

[AOAX](https://aoax.co.uk), a UK-based AI governance practice specialising in
regulated deployments of AI agent systems.

To discuss your specific deployment, book a call at
[aoax.co.uk](https://aoax.co.uk).

---

## Licence

MIT. See [LICENSE](LICENSE.md).

---

## Disclaimer

This repo does not constitute legal advice. It does not guarantee compliance
with any regulation. Regulatory obligations depend on your specific processing
activities, sector, and organisational context.

Use this material as a starting point and engage qualified legal counsel and a
Data Protection Officer for your specific situation before going live with
personal data.
