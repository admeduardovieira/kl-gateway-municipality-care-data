# REVERSE_ENGINEERING_REPORT.md

**Repository:** kl-gateway-municipality-care-data  
**URL:** https://github.com/hl7dk/kl-gateway-municipality-care-data  
**Base branch analyzed:** master (commit 63d089d08b9ebec7d94a216551c3605169577403)  
**Target fork:** https://github.com/admeduardovieira/kl-gateway-municipality-care-data  
**Audit date:** 2026-11-08  
**Environment:** Windows, PowerShell 5.1, Git 2.49.0 (SUSHI not installed)  
**Agent:** Kilo (REA 6.7.0), Git identity admeduardovieira / admeduardovieira@users.noreply.github.com  

---

## A. General diagnosis

### Identified architecture
kl-gateway-municipality-care-data is an FHIR R4 Implementation Guide (IG) for KL Gateway Municipal Care Data, developed by hl7dk (Kommunernes Landsforening, Denmark). Built with SUSHI/FSH and published with IG Publisher. Not an executable application: FSH definitions, invariants, documentation pages, and IG Publisher build scripts.

Structure:
- `input/fsh/*.fsh` — 27 FSH definitions (profiles, extensions, value sets, examples: citizen.fsh, condition.fsh, encounter.fsh, goal.fsh, observation, intervention, deliveryReport, etc.)
- `input/pagecontent/index.md` — home page
- `input/ignoreWarnings.txt` — suppressed warning messages
- `sushi-config.yaml` — SUSHI configuration (id, canonical, dependencies, releaseLabel)
- `ig.ini` — IG Publisher configuration
- `_gencontinuous.bat/.sh`, `_genonce.bat/.sh`, `_updatePublisher.bat/.sh` — IG Publisher build scripts
- `README.md` — minimal (only link to build.fhir.org)

### Technologies and dependencies
- FHIR R4 (4.0.1)
- SUSHI (FSH)
- Dependencies: `hl7.fhir.dk.core` (pinned at 1.1.0), `dk.fhir.ig.kl.common.caresocial: current` (not pinned)
- `ig.ini`: `template = fhir.base.template#current` (template not pinned)
- releaseLabel: `ci-build` (correct spelling)

### Maintenance state
Short history (10 commits). Last commit `63d089d` ("Update README.md"). Many "Update README.md" commits. Last FSH content change: `fff5330` ("Update citizen.fsh"). Low-frequency active project.

### Existing test mechanisms
No automated tests found (no test folder or framework).
No CI/CD (absence of `.github/workflows`; only build.fhir.org link in README).

### Investigation limitations
- SUSHI not installed; impossible to compile/validate `.fsh` files or generate the IG.
- GitHub issues/PRs not accessed beyond the clone content.
- Unable to verify specific versions available in the FHIR registry for pinning dependencies.

---

## B. Problems found

| ID | Title | Severity | Status | Location |
|----|-------|----------|--------|----------|
| REA-016 | Unpinned dependency (`dk.fhir.ig.kl.common.caresocial: current`) | P2 | Proposal (not implemented — lacks version evidence) | `sushi-config.yaml:22` |
| REA-017 | IG Publisher template not pinned (`fhir.base.template#current`) | P2 | Proposal (not implemented — lacks version evidence) | `ig.ini:3` |
| REA-018 | Absence of CI/CD and automated tests | P3 | Proposal (not implemented) | Repository root |
| REA-019 | Nearly empty README (missing install/build/test/contribute instructions) | P3 | Proposal (not implemented) | `README.md` |

### Details

**REA-016 — Unpinned dependency (PROPOSAL)**
- Problem: `sushi-config.yaml` declares `dk.fhir.ig.kl.common.caresocial: current`.
- Evidence: `sushi-config.yaml:18-22`. (`hl7.fhir.dk.core` is pinned at 1.1.0, good practice, but `dk.fhir.ig.kl.common.caresocial` is not.)
- Root cause: version not pinned in SUSHI configuration.
- Impact: non-reproducible builds; risk of breakage by retroactively incompatible upstream change.
- Proposed fix: replace `current` with a specific stable version after verifying availability in the FHIR registry.

**REA-017 — Template not pinned (PROPOSAL)**
- Problem: `ig.ini` declares `template = fhir.base.template#current`.
- Evidence: `ig.ini:3`.
- Root cause: IG Publisher template version not pinned.
- Impact: builds may use different template versions, producing inconsistent output.
- Proposed fix: replace `current` with a specific stable version after verifying availability.

**REA-018 — Absence of CI/CD and automated tests (PROPOSAL)**
- Problem: no `.github/workflows`, no tests, no package.json.
- Evidence: absence of `.github` directory and test files.
- Root cause: the IG has no continuous integration pipeline.
- Impact: FSH changes not automatically validated.
- Proposed fix: add CI that runs SUSHI and validates the IG (requires SUSHI on runner).

**REA-019 — Nearly empty README (PROPOSAL)**
- Problem: `README.md` contains only a link to build.fhir.org, no install/build/test/contribute instructions.
- Evidence: `README.md` (6 lines, 4 blank).
- Root cause: insufficient documentation.
- Impact: hinders contributor onboarding.
- Proposed fix: expand README with installation, build, test, and contribution instructions.

---

## C. Recurring problems

No publicly verified recurring problems (scope limited to clone). The project is an IG with short history.

---

## D. Undocumented errors

Errors identified by the audit that do not appear in documentation:
- REA-016, REA-017, REA-018, REA-019.

---

## E. Performance

No executable code nor runtime operations. Opportunities relate to IG generation time (SUSHI build), not measurable without SUSHI installed.

---

## F. Technical improvements

| Area | Improvement | Justification |
|------|-------------|---------------|
| Supply chain | Pin `dk.fhir.ig.kl.common.caresocial` version and `fhir.base.template` template | Reproducible builds |
| Quality | Add CI/CD with SUSHI/IG validation | Early detection of regressions |
| Documentation | Expand `README.md` with install/build/test/contribute instructions | Contributor onboarding |
| Testability | Add validation tests for FSH profiles and examples | Coverage of behaviors |

---

## G. Execution plan

Independent corrections:
1. **REA-016** (P2): pin dependency version — blocked by lack of version evidence (SUSHI not installed).
2. **REA-017** (P2): pin template version — blocked by lack of version evidence.
3. **REA-018** (P3): add CI/CD — medium (requires SUSHI on runner).
4. **REA-019** (P3): expand README — low, implementable.

Recommended order: REA-019 → REA-018 → REA-016 → REA-017 (the latter two pending version verification).

---

## H. Expected result

- Reproducible IG builds (versions pinned).
- Automatic regression detection via CI.
- Repository with adequate documentation for contributors.
- Audit documentation contribution suitable for a small reviewable PR.

---

## Mandatory final questions

1. **Which critical bugs were confirmed?** None P0/P1.
2. **Which recurring problems have developers not yet solved?** None verified publicly.
3. **Which important errors were not documented?** Unpinned dependency and template, absence of CI/CD/tests, empty README.
4. **Which performance opportunities were found?** None measurable without SUSHI.
5. **Which security and reliability improvements are justifiable?** Pin versions (supply chain), CI/CD.
6. **Which corrections can be made quickly?** REA-019 (expand README).
7. **Which are the three contributions with highest potential technical value?** (1) Pin dependency/template versions (reproducibility), (2) Add CI/CD with SUSHI (regression detection), (3) Expand README (onboarding).
8. **Which corrections should be avoided due to lack of evidence:** Pinning specific dependency/template versions without verifying FHIR registry availability.
9. **Which modifications do you recommend implementing first?** REA-019 (expand README), then REA-018 (CI/CD), then REA-016/REA-017 after version verification.

---

## Malware and threat analysis (static)

Performed on all files (FSH, YAML, INI, MD, BAT, SH), excluding `.git`.

**Result: No indicator identified by the static analysis performed.**

- `.bat`/`.sh` scripts are legitimate IG Publisher build scripts (publisher.jar), ping/curl `tx.fhir.org` to check connectivity — expected behavior.
- URLs/domains are legitimate (hl7.dk, fhir.org, build.fhir.org).
- No embedded key, credential, token, or password.
- No obfuscation, packing, persistence, suspicious process execution, data collection/exfiltration.
- Limitation: static analysis only. It is not stated that the project is free of malware.

---

**Note:** This report is the Portuguese base version. Synchronized English (`.en.md`) and Ukrainian (`.uk.md`) versions accompany this document, preserving REA-IDs, P0–P3 severities, paths, commands, URLs, and code snippets unchanged.
