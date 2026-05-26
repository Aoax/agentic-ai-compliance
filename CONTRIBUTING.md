# Contributing

|                    |             |
| ------------------ | ----------- |
| **Version**        | 0.1         |
| **Status**         | Draft       |
| **Owner**          | Dave Butler |
| **Updated**        | 2026-05-21  |
| **Classification** | Public      |

Contributions are welcome. This repo improves when practitioners share what they
encounter in real agentic AI deployments, across any orchestrator.

---

## What contributions are welcome

- New framework mappings (ISO 42001, NIST CSF, EU AI Act, sector-specific)
- Sector-specific worked examples (fintech, legal, healthcare, public sector)
- Gap analysis updates when an orchestrator release closes or introduces a gap
- Runbook improvements based on real deployment experience
- Corrections to existing mappings where the regulatory analysis is wrong or out
  of date

---

## What to include in a pull request

Every PR that adds or changes regulatory content should include:

- The regulatory basis for any claim: cite the specific article, recital, or
  guidance document
- A worked example if you are adding a new control mapping or gap entry
- A note on which orchestrator and version the content applies to (or
  "orchestrator agnostic" if it is a governance control rather than a technical
  one)

Do not include legal advice. Describe what the regulation requires and what the
orchestrator does or does not do. Leave interpretation of how that applies to a
specific organisation's situation to qualified counsel.

---

## Review process

AOAX reviews all PRs for regulatory accuracy before merging. This may take a few
days. If your PR is urgent (for example, a gap introduced by a recent
orchestrator release), flag it in the PR description.

If you are unsure whether a contribution is appropriate, open an issue first.

---

## Style

- UK English
- No em dashes, no emojis, no marketing language
- Tables where they aid clarity, checkboxes for checklists
- Keep it practical: write for a developer or DevOps engineer who needs to act
  on the information, not for an academic audience
