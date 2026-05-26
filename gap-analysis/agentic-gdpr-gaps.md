# GDPR Gap Analysis for Agentic AI Deployments — Reference Implementation: Paperclip

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Draft       |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

The gaps documented here are common across agentic AI orchestrators. The
Paperclip reference implementation is used as the concrete example because it is
the most thoroughly reviewed, but teams using n8n, LangGraph, CrewAI, custom
Claude API agents, or other orchestrators should expect to encounter the same
gaps in their own stack.

This register reflects Paperclip as of May 2026. Check this repo for updates
after each release, and open a PR if you find a gap that has been closed or a
new one that should be listed.

This is not a criticism of any specific orchestrator. It is a practical register
for teams who need to know what they are responsible for building or documenting
themselves before they go live with personal data.

---

## Gap register

| Gap                                                 | Risk                                                                                                                                                                    | Workaround                                                                                                                                                                        | Upstream status                     |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| No audit log export mechanism                       | Cannot produce a structured evidence export for regulatory review or data subject access requests (Art. 15, Art. 5(1)(f))                                               | Supplement the orchestrator's internal log (e.g. Paperclip's) with Postgres-level audit logging. Follow `runbooks/deployment-checklist.md` section 5. Test export before go-live. | Not planned in Paperclip (May 2026) |
| No right to erasure workflow                        | Art. 17 non-compliance: personal data accumulates across agent outputs, logs, and cached prompt data with no deletion path                                              | Write and test a manual erasure runbook covering all tables containing personal data. Use `runbooks/erasure-runbook.md` as a starting point.                                      | Not planned in Paperclip (May 2026) |
| No data residency controls                          | Personal data may leave the UK or EU via LLM API calls with no transfer mechanism in place (Art. 44)                                                                    | Review each LLM provider's DPA and transfer safeguards. Obtain SCCs or IDTA before processing personal data. List providers in the sub-processor schedule.                        | Not planned in Paperclip (May 2026) |
| No RBAC or multi-user access controls               | Many agentic orchestrators, including Paperclip, share database credentials across agents by default. No least-privilege enforcement; one compromised agent affects all | Implement Postgres row-level security per agent. Issue separate database credentials per agent or agent group. Document access controls in the ROPA.                              | Not planned in Paperclip (May 2026) |
| No automated data deletion or retention enforcement | Art. 5(1)(e) storage limitation: data accumulates indefinitely with no mechanism to enforce documented retention periods                                                | Implement a scheduled Postgres cleanup job that deletes records past their retention date. Document the job, its schedule, and the retention periods it enforces.                 | Not planned in Paperclip (May 2026) |
| No DPIA trigger mechanism                           | Art. 35: high-risk processing may begin without a completed DPIA                                                                                                        | Use the DPIA template in this repo. Assess each deployment against the ICO's high-risk criteria before go-live. Record the assessment outcome with the ROPA entry.                | Not planned in Paperclip (May 2026) |
| No sub-processor disclosure                         | Art. 28(3)(c): controllers using agentic AI products may be unaware that LLM providers are processing their data                                                        | Document all LLM providers in the DPA sub-processor schedule. Notify controllers before adding or changing providers. Provide a mechanism for controllers to object.              | Not planned in Paperclip (May 2026) |
| No human-in-the-loop for consequential decisions    | Art. 22: many agentic orchestrators, including Paperclip, can make decisions with legal or similarly significant effects on individuals with no human review step       | Implement a mandatory approval gate in workflow configuration for any agent that produces consequential decisions. Document the gate as a technical and organisational measure.   | Not planned in Paperclip (May 2026) |
| No privacy notice generation                        | Art. 13/14: data subjects may not be informed that their data is processed by AI agents                                                                                 | Use the privacy notice template in this repo. Publish before processing begins. Update whenever agent scope changes materially.                                                   | Not planned in Paperclip (May 2026) |

---

## How to contribute gap updates

If you discover a new gap in any agentic AI deployment, open a pull request
adding a row to the table above. Include:

- The specific regulatory obligation at risk
- The practical consequence if the gap is not addressed
- A concrete workaround based on what you have implemented
- The orchestrator and version you observed the gap in (e.g. Paperclip 1.2, n8n
  1.x, LangGraph 0.x)

If a release closes a gap listed above, open a PR updating the "Upstream status"
column and, if the workaround is no longer necessary, noting that in the
Workaround column.

See [CONTRIBUTING.md](../CONTRIBUTING.md) for the full contribution guide.
