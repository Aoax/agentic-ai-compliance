# Agentic AI Compliance Framework

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Draft       |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

This repository provides compliance mappings, deployment checklists, and gap
analyses for regulated organisations deploying AI agent orchestrators. It covers
Paperclip (primary reference implementation), n8n, custom Claude API agents, and
emerging orchestrators.

---

## Why this exists

AI agent orchestrators: Paperclip, n8n, LangGraph, CrewAI, custom Claude API
agents: have no built-in compliance frameworks. Their documentation does not
address the obligations that apply when you process personal data, operate in
regulated sectors, or need to demonstrate governance to auditors.

This repo fills that gap. It maps orchestrator capabilities against UK and EU
GDPR today, with the EU AI Act, ISO 42001, and NIST CSF planned. Paperclip is
the primary reference implementation; mappings note where controls are orchestrator-agnostic and where
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

The worked example covers a healthcare processor scenario. Sector-specific
notes for fintech (FCA), legal (SRA), and the rest of healthcare (DSPT, ICO
Children's Code) are planned, not written.

---

## What this covers today

| Framework | Scope                                                            | Status  |
| --------- | ---------------------------------------------------------------- | ------- |
| UK GDPR   | Article-by-article control mapping, gap register, worked example | Written |
| EU GDPR   | As UK GDPR; divergences noted where relevant                     | Written |
| EU AI Act | Risk classification guidance for agentic AI use cases            | Planned |
| ISO 42001 | AI management system controls mapped to agentic deployments      | Planned |
| NIST CSF  | Cybersecurity framework controls for the hosting infrastructure  | Planned |

Planned means not written. Nothing in this repo covers those three yet, and
the mapping below cites UK GDPR only. Don't plan an audit around a row marked
planned.

---

## How to use it

1. Read `frameworks/gdpr/control-mapping.md` to understand your baseline
   obligations and your orchestrator's current gaps.
2. Work through `runbooks/deployment-checklist.md` before go-live. Every
   checkbox is there for a reason.
3. Review `gap-analysis/agentic-gdpr-gaps.md` and decide which workarounds you
   will implement.
4. Read `frameworks/gdpr/worked-example-health-authority-processor.md` if you
   are operating as a processor; it is the only worked example so far.

Start with the checklist. It will surface the documents and decisions you need.

---

## Repo structure

What exists:

```text
agentic-ai-compliance/
  frameworks/
    gdpr/
      control-mapping.md          # Article-by-article UK GDPR mapping
      worked-example-health-authority-processor.md
  runbooks/
    deployment-checklist.md       # Pre-go-live compliance checklist
  gap-analysis/
    agentic-gdpr-gaps.md          # Gap register with workarounds
```

What does not exist yet, and is referenced from the documents above as a
next step rather than as something you can open today: an incident-response
runbook, an erasure runbook, and templates for a DPA, ROPA, DPIA, and an
Art. 13/14 privacy notice. Each is named in the control mapping or the
deployment checklist as the mitigation for a gap. Until they are written,
that mitigation is a description of work you have to do yourself, not a
document you can pick up. Treat those references accordingly.

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
