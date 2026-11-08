# CORRECTIONS.md

**Repository:** kl-gateway-municipality-care-data  
**URL:** https://github.com/hl7dk/kl-gateway-municipality-care-data  
**Base branch analyzed:** master (commit 63d089d08b9ebec7d94a216551c3605169577403)  
**Target fork:** https://github.com/admeduardovieira/kl-gateway-municipality-care-data  
**Date:** 2026-11-08  
**Agent:** Kilo (REA 6.7.0), Git identity admeduardovieira / admeduardovieira@users.noreply.github.com  

---

## Proposed corrections and status

| ID | Title | Severity | Status | Location | Risk | Suggested PR |
|----|-------|----------|--------|----------|------|--------------|
| REA-016 | Pin `dk.fhir.ig.kl.common.caresocial` version | P2 | Proposal (not implemented — lacks version evidence) | `sushi-config.yaml:22` | Low | `fix/rea-016-pin-dependency` |
| REA-017 | Pin `fhir.base.template#current` template version | P2 | Proposal (not implemented — lacks version evidence) | `ig.ini:3` | Low | `fix/rea-017-pin-template` |
| REA-018 | Add CI/CD and automated tests | P3 | Proposal (not implemented) | Repository root | Medium | `ci/rea-018-add-sushi-ci` |
| REA-019 | Expand `README.md` with install/build/test/contribute instructions | P3 | Proposal (not implemented) | `README.md` | Low | `docs/rea-019-expand-readme` |

---

## Details per correction

### REA-016 — Pin dependency version

**Problem:** `sushi-config.yaml` declares `dk.fhir.ig.kl.common.caresocial: current`.

**Evidence:** `sushi-config.yaml:22`.

**Root cause:** version not pinned.

**Proposed fix:** replace `current` with a specific stable version after verifying availability in the FHIR registry.

**Files involved:** `sushi-config.yaml`

**Validation test:** validate YAML with a parser and confirm no `current` value remains in dependencies.

**Risk:** low.

**Benefit:** reproducible builds.

**Priority:** immediate (pending version verification).

---

### REA-017 — Pin template version

**Problem:** `ig.ini` declares `template = fhir.base.template#current`.

**Evidence:** `ig.ini:3`.

**Root cause:** template version not pinned.

**Proposed fix:** replace `current` with a specific stable version.

**Files involved:** `ig.ini`

**Validation test:** confirm IG Publisher accepts the pinned version.

**Risk:** low.

**Benefit:** consistent build output.

**Priority:** immediate (pending version verification).

---

### REA-018 — Add CI/CD and automated tests

**Problem:** absence of `.github/workflows` and tests.

**Evidence:** absence of `.github` directory and test files.

**Root cause:** the IG has no continuous integration pipeline.

**Proposed fix:** add a GitHub Actions workflow that runs SUSHI and validates the IG.

**Files involved:** `.github/workflows/ci.yml` (new).

**Validation test:** run the workflow on fork/branch and confirm successful completion.

**Risk:** medium (requires SUSHI on CI runner).

**Benefit:** early detection of regressions.

**Priority:** medium.

---

### REA-019 — Expand `README.md`

**Problem:** `README.md` contains only a link to build.fhir.org.

**Evidence:** `README.md` (6 lines).

**Root cause:** insufficient documentation.

**Proposed fix:** add installation, build, test, and contribution instructions.

**Files involved:** `README.md`

**Validation test:** confirm README contains expected sections.

**Risk:** low.

**Benefit:** contributor onboarding.

**Priority:** immediate.

---

## Uncorrected problems and residual risks

- All corrections are in "Proposal" status at this audit stage (Phase 1 completed). No change has been applied to source code yet.
- Limitation: SUSHI not installed; unable to verify specific versions available (REA-016, REA-017) nor validate CI (REA-018).
- Residual risks: unpinned dependency and template, absence of CI/CD/tests, empty README.

---

## Report records

- Base PT: `REVERSE_ENGINEERING_REPORT.md` and `CORRECTIONS.md` (this file).
- Translations: `REVERSE_ENGINEERING_REPORT.en.md`, `REVERSE_ENGINEERING_REPORT.uk.md`, `CORRECTIONS.en.md`, `CORRECTIONS.uk.md`.
- All at the root of the working clone / audit area, alongside the Portuguese base reports.

**Translation note (uk):** Machine-assisted technical translation; native speaker review recommended before use as a normative reference. No native review was obtained.
