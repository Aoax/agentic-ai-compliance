# Deployment Checklist: Pre-Go-Live Compliance

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Draft       |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

Work through this checklist before processing any real personal data on an
agentic AI stack. Every item is there because a real regulatory obligation or
audit finding sits behind it.

Steps marked "(Paperclip reference implementation)" apply specifically to
Paperclip. Teams using other orchestrators should apply the equivalent control
for their stack.

This checklist covers the baseline for a UK GDPR-regulated deployment. If you
are in a specific sector (fintech, healthcare, legal), check the relevant
framework directory for additional requirements.

---

## 1. Before you start

Legal and governance prerequisites. Do not deploy to production until these are
in place.

- [ ] A Data Protection Officer (DPO) has been engaged, or a formal assessment
      has been completed and documented confirming that a DPO is not required
      for this deployment
- [ ] A Data Protection Impact Assessment (DPIA) has been triggered and is
      underway or complete for any processing that is likely to result in high
      risk to individuals (Art. 35)
- [ ] The lawful basis for each category of processing has been identified and
      documented (Art. 6, and Art. 9 where special category data is involved)
- [ ] If operating as a data processor: a signed Data Processing Agreement (DPA)
      is in place with each data controller before any of their data is
      processed (Art. 28)
- [ ] A named individual is responsible for data protection compliance for this
      deployment

---

## 2. Infrastructure

- [ ] The VPS or cloud instance hosting the agentic stack has encryption at rest
      enabled at the volume or storage level
- [ ] HTTPS/TLS is enforced for all external-facing endpoints; plain HTTP
      connections are rejected at the nginx layer
- [ ] Firewall rules restrict inbound access to the minimum necessary ports; the
      orchestrator's internal API is not reachable from the public internet
- [ ] The orchestrator UI is not publicly exposed; access is restricted by IP
      allowlist, VPN, or equivalent (Paperclip reference implementation: applies
      to the Paperclip UI)
- [ ] Postgres is not accessible from the public internet; connections are
      restricted to the application server and authorised administrators
- [ ] Database backups are encrypted and stored in a location with access
      controls equivalent to the primary instance
- [ ] SSH access to the server is key-based; password authentication is disabled

---

## 3. Data protection

- [ ] A Records of Processing Activities (ROPA) entry has been created for this
      agentic deployment (Art. 30)
- [ ] A data retention policy is documented, specifying how long each category
      of personal data will be kept and the basis for that period
- [ ] An erasure runbook has been written and tested: it covers all tables and
      stores containing personal data, including the orchestrator's internal
      tables, your application database, and any LLM prompt caches (Art. 17)
      (Paperclip reference implementation: includes Paperclip's internal tables)
- [ ] A Subject Access Request (SAR) response process is defined: who handles
      requests, what data will be returned, how long it will take (Art. 15)
- [ ] Privacy notices are published covering the processing carried out by
      agentic AI, in accordance with Art. 13/14

---

## 4. Access controls

- [ ] A list of individuals with access to the agentic AI deployment is
      documented and up to date
- [ ] Credentials are managed via a password manager or secrets manager; no
      credentials are stored in plaintext in config files, environment files
      committed to version control, or chat logs
- [ ] Postgres row-level security (RLS) is configured so that each agent has
      access only to the data categories it is authorised to process
- [ ] Separate database credentials are in use per agent or agent group; no
      single shared credential with broad access
- [ ] Access to audit logs is restricted to authorised personnel; agents cannot
      modify or delete audit records

---

## 5. Audit logging

- [ ] The orchestrator's internal audit log is enabled and confirmed to be
      writing records (Paperclip reference implementation: Paperclip's internal
      audit log)
- [ ] A supplementary Postgres-level audit log is configured, capturing at
      minimum: timestamp, agent identifier, operation type, and affected record
      references
- [ ] Log retention period is set, documented, and consistent with the data
      retention policy
- [ ] Audit log export has been tested: you can produce a log extract covering a
      specified time period and agent in a format suitable for regulatory review
- [ ] Log storage is sized and monitored so that logs are not silently dropped
      due to disk pressure

---

## 6. Third-party and sub-processor review

- [ ] A DPA has been signed with each LLM API provider whose API receives
      personal data from the agentic deployment (Art. 28)
- [ ] For any LLM provider based outside the UK or EU: a valid international
      transfer mechanism is in place (SCCs, IDTA, or adequacy decision) before
      personal data is sent to that provider (Art. 44)
- [ ] A sub-processor list is documented and included in or appended to your
      DPA: provider name, processing country, data categories processed, and
      transfer safeguard
- [ ] If operating as a processor: each controller has been notified of the
      sub-processor list and given the opportunity to object before processing
      begins (Art. 28(3)(c))

---

## 7. Agent configuration

- [ ] Each agent's scope is documented in writing: what data it is authorised to
      access, what actions it can take, and what it must not do
- [ ] Budget caps or rate limits are configured to limit the volume of data an
      agent can process in a given period
- [ ] For any agent that produces decisions with legal or similarly significant
      effects on individuals: a mandatory human review step is implemented
      before the decision is actioned (Art. 22)
- [ ] Agent prompt templates have been reviewed to confirm that personal data
      entering LLM context is limited to what is necessary for the task
- [ ] Agent configurations are version-controlled; changes to agent scope are
      logged and reviewed

---

## 8. Incident response

- [ ] A breach response process is documented: how a breach is detected, who is
      notified internally, and who is responsible for ICO notification
- [ ] The 72-hour ICO notification deadline is understood by all relevant staff:
      the clock starts from when the organisation becomes aware of the breach,
      not when it is confirmed (Art. 33)
- [ ] A process is defined for notifying affected individuals when a breach is
      likely to result in high risk to their rights and freedoms (Art. 34)
- [ ] If operating as a processor: the process for notifying the controller
      without undue delay is documented and the controller's notification
      contact is recorded
- [ ] Contact details for the ICO are accessible to the person responsible for
      breach notification:
      [ico.org.uk/report-a-breach](https://ico.org.uk/report-a-breach)

---

## 9. Go-live sign-off

- [ ] All items above have been reviewed and either completed or have a
      documented exception with an accepted risk owner
- [ ] The completed checklist has been signed off by a named individual with
      authority to approve go-live
- [ ] The sign-off record includes: name, role, date, orchestrator name and
      version, and deployment identifier
- [ ] The signed checklist is stored with the ROPA entry for this deployment and
      retained for the duration of the processing plus applicable limitation
      period

---

_Maintained by [AOAX](https://aoax.co.uk). Raise issues or improvements via pull
request._
