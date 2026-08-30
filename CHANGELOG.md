# Changelog

All notable changes to this repository are documented in this file.

## [0.1.1] - 2026-08-30

### Changed

- README now states which frameworks are written and which are planned. The
  coverage table previously advertised UK GDPR, EU GDPR, the EU AI Act, ISO
  42001, and NIST CSF; only the GDPR mappings exist.
- The repo structure section lists the files that exist, and names the six
  planned documents (incident-response and erasure runbooks, DPA, ROPA, DPIA,
  and privacy notice templates) as planned rather than as files a reader can
  open.
- Every mitigation that pointed at one of those unwritten documents now says
  so, in the control mapping, the gap register, the worked example, and the
  deployment checklist. The obligation stands; the template does not exist yet.
- The gap register explains what "not planned in Paperclip" is based on: no
  published issue or roadmap entry could be found, which is not the same as a
  statement about anyone's intentions.
- Status in the README is Draft, matching every other document in the repo.
- Removed em dashes, per the house style in CONTRIBUTING.md.

### Added

- `.cspell.json` (en-GB), `.markdownlint.json`, `CHANGELOG.md`, `VERSION`.
