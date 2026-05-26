# UK GDPR Control Mapping: Agentic AI Deployments

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Draft       |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

This document maps UK GDPR obligations to the capabilities of agentic AI
deployments, identifies common gaps, and documents mitigations. It covers the
articles most likely to be relevant to any agentic deployment processing
personal data on behalf of regulated UK organisations.

The reference implementation is Paperclip (open-source, self-hosted). Where
capability notes reference a specific implementation, Paperclip is used as the
concrete example. Teams using n8n, LangGraph, CrewAI, custom Claude API agents,
or other orchestrators should apply the same controls to their chosen stack.

This mapping reflects the Paperclip reference implementation as of May 2026.
Check the gap register (`gap-analysis/agentic-gdpr-gaps.md`) for updates.

---

## Control mapping

| Article      | Requirement                                                                                               | Orchestrator Native (Paperclip reference)                      | Gap                                                                                                           | Mitigation                                                                                                                       |
| ------------ | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Art. 5(1)(a) | Lawfulness, fairness, transparency: data processed lawfully, fairly, and in a transparent manner          | None                                                           | No mechanism to document or enforce lawful basis at the agent or workflow level                               | Document lawful basis in ROPA; configure agents only for documented purposes; reference in DPA if operating as processor         |
| Art. 5(1)(b) | Purpose limitation: data collected for specified purposes, not processed in incompatible ways             | None                                                           | Agents can be prompted to use data for any purpose reachable in the database                                  | Document agent scope in writing; use Postgres row-level security to limit data access to authorised categories                   |
| Art. 5(1)(c) | Data minimisation: data adequate, relevant, and limited to what is necessary                              | None                                                           | No controls on what data enters agent context or LLM prompts                                                  | Audit prompt templates; pseudonymise or strip identifiers where not necessary; review data passed to LLM APIs                    |
| Art. 5(1)(d) | Accuracy: personal data kept accurate and up to date                                                      | None                                                           | Agent outputs may contain inaccurate inferences; no correction workflow                                       | Do not treat agent outputs as authoritative records; implement human review for any data that will be stored or acted upon       |
| Art. 5(1)(e) | Storage limitation: data not kept longer than necessary                                                   | None                                                           | Agent outputs, logs, and cached data accumulate with no automatic deletion                                    | Document retention periods; implement scheduled Postgres cleanup job; test and log deletions                                     |
| Art. 5(1)(f) | Integrity and confidentiality: appropriate security for personal data                                     | TLS on outbound API calls; internal audit log                  | No enforced encryption at rest; no RBAC; all agents share DB credentials by default                           | Encrypt Postgres volume at VPS level; implement Postgres row-level security; separate DB credentials per agent                   |
| Art. 6       | Lawful basis: processing must have a valid lawful basis                                                   | None                                                           | No mechanism to record or enforce lawful basis; the agent orchestrator will process whatever data it receives | Document lawful basis in ROPA before deployment; gate data ingestion on confirmed lawful basis in your application layer         |
| Art. 9       | Special category data: additional restrictions on health, biometric, political, religious, and other data | None                                                           | No restrictions on what data agents can access or process; special category data treated as any other         | Implement Postgres row-level security to restrict agent access to special category tables; document in DPA; review agent prompts |
| Art. 13/14   | Transparency: data subjects must be informed about processing                                             | None                                                           | No privacy notice generation; no mechanism to surface agent processing to data subjects                       | Use privacy notice template in this repo; publish before processing begins; update when agent scope changes                      |
| Art. 17      | Right to erasure: data subjects may request deletion of their data                                        | None                                                           | No built-in erasure workflow; agent outputs, logs, and cached data accumulate across multiple tables          | Write and test an erasure runbook; identify all tables containing personal data; use `runbooks/erasure-runbook.md`               |
| Art. 20      | Data portability: data subjects may request their data in a portable format                               | None                                                           | No export mechanism for agent-processed data or outputs                                                       | Implement a data export query in your application layer; document the process in your ROPA                                       |
| Art. 22      | Automated decision-making: safeguards for decisions with legal or similarly significant effects           | None                                                           | Agents make decisions autonomously by default; no human-in-the-loop mechanism                                 | Implement approval gates in workflow configuration for any consequential decisions; document as a TOM                            |
| Art. 25      | Data protection by design and default: privacy built in, not bolted on                                    | None                                                           | No privacy-by-default configuration; maximum data access is the default                                       | Apply least-privilege via Postgres RLS from the start; do not grant broad access and restrict later                              |
| Art. 28      | Processor obligations: DPA required when processing on behalf of a controller                             | None                                                           | No DPA generation or sub-processor management tooling                                                         | Use DPA template in this repo; sign before processing any controller's data; maintain sub-processor schedule                     |
| Art. 30      | Records of processing: controller and processor must maintain ROPA                                        | None                                                           | No ROPA generation or structured record of what agents process                                                | Use ROPA template in this repo; create an entry for each agentic deployment before go-live                                       |
| Art. 32      | Security of processing: technical and organisational measures appropriate to the risk                     | TLS on outbound calls; basic API authentication; internal logs | No enforced encryption at rest; no RBAC; no access controls beyond API key; no audit log export               | VPS-level encryption; Postgres RLS; separate credentials per agent; supplement with Postgres audit logging                       |
| Art. 33/34   | Breach notification: notify ICO within 72 hours; notify affected individuals where high risk              | None                                                           | No breach detection, alerting, or notification workflow                                                       | Implement log monitoring and alerting; document incident response process; use `runbooks/incident-response.md`                   |
| Art. 35      | DPIA: required before high-risk processing                                                                | None                                                           | No DPIA trigger or template; no mechanism to flag high-risk processing configurations                         | Complete DPIA before go-live for any high-risk use case; use DPIA template in this repo                                          |
| Art. 44      | International transfers: personal data may not be transferred outside UK/EU without a valid mechanism     | None                                                           | LLM API calls may send personal data to US or other third-country providers with no transfer mechanism        | Review LLM provider DPA; obtain SCCs or IDTA before processing personal data; list providers in sub-processor schedule           |

---

## What the Paperclip reference implementation does well

This mapping focuses on gaps, but a fair assessment acknowledges what Paperclip
gets right as the reference implementation. Teams evaluating other orchestrators
should ask equivalent questions of their chosen stack.

**Audit logging.** Paperclip maintains an internal audit log of agent activity.
It is not exportable in a guaranteed format, but it exists and is queryable.
Combined with Postgres-level audit logging, it provides a reasonable evidence
trail. Check whether your orchestrator provides equivalent built-in logging.

**Open-source codebase.** You can read exactly what Paperclip does with your
data. There are no hidden data flows, no telemetry sent to a vendor, and no
black-box behaviour. This is a significant advantage over SaaS AI products where
data handling is opaque. For other open-source orchestrators (n8n, LangGraph,
CrewAI), the same scrutiny applies; for proprietary products, rely on DPA
disclosures and audit reports.

**Data ownership.** You run Paperclip on your own infrastructure. Your data
stays in your Postgres instance. There is no mandatory cloud dependency and no
vendor lock-in for data at rest. Self-hosted orchestrators generally share this
property; managed/SaaS orchestrators do not.

**Active development.** The Paperclip community is active. Gaps documented here
may be closed by future releases. Check the gap register and the Paperclip
changelog when you upgrade.

**Architectural flexibility.** The compliance gaps documented above are gaps in
documentation and tooling, not fundamental architectural problems. Paperclip's
architecture does not prevent you from implementing RBAC, audit logging, erasure
workflows, or any other control. It simply does not provide them out of the box.
The same is broadly true of other open-source orchestrators.
