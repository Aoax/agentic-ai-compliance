# Worked Example: Healthtech Operator as Data Processor for a Health Authority

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Draft       |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

## Scenario

A healthtech platform uses Paperclip AI agents to process patient documents on
behalf of a Health Authority (HA). The HA is the Data Controller. The healthtech
operator is the Data Processor.

Typical agent tasks in this scenario:

- Summarising patient referral letters
- Drafting administrative correspondence on behalf of the HA
- Routing documents to the appropriate care pathway

All data processed belongs to the HA's patients. The operator determines none of
the purposes; it acts only on the HA's documented instructions.

---

## Applicable GDPR Obligations

### Article 9: Special Category Data

Health data is special category data. The operator does not choose the lawful
basis — that is the HA's responsibility as DC. The operator must:

- Process only under documented instructions from the HA
- Implement suitable technical and organisational measures (TOMs)
- Not process for any purpose outside the HA's documented instructions

**Paperclip gap:** No mechanism to restrict agent processing to a defined
instruction set at the data layer. An agent can be prompted to access any data
it can reach in the database.

**Mitigation:** Document agent scope restrictions in the Data Processing
Agreement (DPA). Implement Postgres row-level security to limit each agent's
database access to the specific data categories covered by the DPA. Maintain an
agent configuration audit trail showing what each agent is authorised to
process.

---

### Article 28 — Processor Obligations

A signed DPA must be in place with the HA before any processing begins. The DPA
must specify:

- Subject matter, duration, nature, and purpose of processing
- Type of personal data and categories of data subjects
- Obligations and rights of the controller

**Paperclip gap:** None. This is a contractual and governance control.

**Required:** Do not process any HA data before the DPA is signed. Use the DPA
template in this repo.

---

### Article 28(3)(c) — Sub-processors and LLM API Calls

Paperclip agents make API calls to LLM providers (Anthropic, OpenAI, or
similar). If patient data appears in agent prompts, the LLM provider is
processing special category health data as a sub-processor.

**This is the most commonly overlooked gap in healthtech Paperclip
deployments.**

Requirements:

- The HA must give prior written authorisation for each sub-processor
- Each sub-processor must offer equivalent GDPR protections via a DPA or
  Standard Contractual Clauses (SCCs)
- The operator must notify the HA of any intended changes to the sub-processor
  list before those changes take effect
- Transfers to US-based LLM providers require a valid transfer mechanism (SCCs
  or adequacy decision)

**Mitigations:**

1. Audit every agent prompt template to identify where patient data enters the
   LLM context
2. Assess whether patient identifiers are necessary in prompts or can be
   pseudonymised before the API call
3. Obtain the LLM provider's DPA and SCCs (both Anthropic and OpenAI publish
   these) and countersign before processing any health data
4. List each LLM provider in the DPA sub-processor schedule: provider name,
   processing country, safeguard mechanism
5. For high-sensitivity deployments: evaluate EU-hosted or on-premises model
   options to avoid international transfer obligations

---

### Article 30(2) — Processor Records of Processing Activities

As a processor, the operator must maintain its own Article 30(2) ROPA entry
covering:

- Name and contact details of the processor and each controller it acts for
- Categories of processing carried out on each controller's behalf
- Transfers to third countries, including LLM API calls
- General description of security measures

**Paperclip gap:** None. This is a governance record.

**Required:** Create an Article 30(2) entry for each HA engagement. Use the ROPA
template in this repo.

---

### Article 32 — Security of Processing

Special category health data requires appropriate technical and organisational
measures. Key controls for a Paperclip deployment:

| Control               | Requirement                                    | Paperclip Native          | Gap                                               | Mitigation                                                                 |
| --------------------- | ---------------------------------------------- | ------------------------- | ------------------------------------------------- | -------------------------------------------------------------------------- |
| Encryption at rest    | Health data in Postgres must be encrypted      | Not enforced              | Postgres volume may be unencrypted                | Enable encryption at rest at the VPS or volume level before deployment     |
| Encryption in transit | All health data in motion must be encrypted    | TLS on outbound API calls | None if nginx handles TLS termination correctly   | Enforce HTTPS everywhere; reject plain HTTP at the nginx layer             |
| Access control        | Only authorised agents may access patient data | Basic API authentication  | No RBAC; all agents share the same DB credentials | Postgres row-level security per agent; separate DB credentials per agent   |
| Pseudonymisation      | Reduce exposure where possible                 | None                      | Patient identifiers pass raw into LLM context     | Pseudonymise patient references in prompts where clinically possible       |
| Audit logging         | Record who accessed what and when              | Internal Paperclip log    | No guaranteed format; no export mechanism         | Supplement with Postgres-level audit logging; follow the audit log runbook |

---

### Article 33 — Breach Notification

As a processor, if you become aware of a personal data breach you must notify
the HA without undue delay. The 72-hour notification clock belongs to the HA as
DC, but any processor delay eats into that window.

**Required:** A documented incident response process. See
`runbooks/incident-response.md`.

---

### Article 22 — Automated Decision-Making

If Paperclip agents produce decisions with legal or similarly significant
effects on patients — routing a referral, flagging a case for escalation, or
prioritising a care pathway — Article 22 applies. In a health context this
threshold is easily crossed.

**HA responsibility (as DC):** Ensure any automated decision-making has a valid
Art. 22 basis: explicit patient consent, necessity for a contract, or statutory
authorisation with suitable safeguards.

**Operator responsibility:** Do not deploy agents that make care-pathway or
escalation decisions without written confirmation from the HA that their Art. 22
obligations are satisfied. Document this explicitly in the DPA scope.

**Paperclip gap:** No built-in human-in-the-loop mechanism. Agents act
autonomously by default.

**Mitigation:** For any agent touching clinical routing or escalation, implement
a mandatory human review step before the decision is actioned. Document this as
a TOM in the DPA.

---

## Deployment Checklist for This Scenario

- [ ] DPA signed with the HA before any patient data is processed
- [ ] Sub-processor schedule complete: all LLM providers listed, SCCs or DPAs
      obtained and countersigned
- [ ] HA has completed a DPIA covering this Paperclip deployment
- [ ] Article 30(2) processor ROPA entry created for this engagement
- [ ] Postgres encryption at rest confirmed at the VPS or volume level
- [ ] Postgres row-level security configured: each agent accesses only
      authorised data categories
- [ ] Agent scope documented in writing: exactly what processing is authorised
      under the DPA
- [ ] Audit log export tested and runbook in place
- [ ] Incident response runbook in place; HA notified of breach notification
      process and contact
- [ ] Any agent touching clinical decisions has a mandatory human review step
      before action

---

## What This Example Does Not Cover

- The operator's own user data (separate controller obligations apply)
- Joint controller arrangements under Article 26
- NHS-specific frameworks: DSPT, DSP Toolkit, Data Security Standards
- ISO 27001 or Cyber Essentials controls (see relevant framework directories)
- EU AI Act risk classification for the agent use cases in this scenario

See `gap-analysis/agentic-gdpr-gaps.md` for a full register of current GDPR gaps
and mitigations.
