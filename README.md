# AI-Test-Case-Generation-with-Anti-Hallucination

A project exploring whether AI-generated test cases become more reliable when an anti-hallucination rule is applied during generation.

---

## What This Project Contains

This project is an experiment around AI-generated test cases for the **VWO Login Dashboard**, comparing generation quality with and without an anti-hallucination rule.

### Files

| Path | Description |
|------|-------------|
| `ch_01_anti_hallucination.md` | The anti-hallucination rule used to constrain AI test-case generation. It defines a strict scope of knowledge, forbids inventing features or behaviour, requires missing information to be reported, and mandates a self-validation step. |
| `requirements/Product Requirements Document_ VWO Login Dashboard.pdf` | The source PRD for the VWO login dashboard (app.vwo.com). |
| `requirements/PRD_VWO_Login_Text.txt` | Text extracted from the PRD PDF, saved so the content can be read without re-extracting it each time. |
| `generated-testcases/vwo_login_testcases.md` | The full VWO login test case suite (132 test cases) generated from the PRD. |
| `generated-testcases/vwo_login_missing_facts.md` | A cross-reference of the test cases against the PRD, listing facts the PRD does not state — the gaps that can cause wrong test cases. |
| `generated-testcases/rate_limiting_testcases_without_rule.md` | Rate-limiting test cases generated **without** the anti-hallucination rule, kept as a baseline for comparison. |

---

## How the Pieces Fit Together

1. The **PRD** (`requirements/`) is the single source of truth. The anti-hallucination rule (`ch_01_anti_hallucination.md`) allows the AI to use only what the PRD states.
2. The full **test case suite** (`generated-testcases/vwo_login_testcases.md`) was generated from the PRD.
3. **Missing facts analysis** (`generated-testcases/vwo_login_missing_facts.md`) checks each test case against the PRD text and flags assertions that are not backed by a stated requirement.
4. The **rate-limiting baseline** (`generated-testcases/rate_limiting_testcases_without_rule.md`) shows what the AI produces when the rule is not applied — the output looks complete but embeds unverified assumptions (attempt thresholds, lockout durations, HTTP status codes, etc.).
5. The same rate-limiting requirement can be regenerated **with** the rule attached; the expected result is a set of test cases that separate verified facts from missing information and label uncertainty explicitly.

---

## Missing Facts Summary

The PRD states requirements at a high level and does not define concrete values. These gaps are detailed in `generated-testcases/vwo_login_missing_facts.md` and include:

- **Password complexity rules** — "enforced security standards" but no minimum length or character requirements
- **2FA method** — "optional 2FA support" but no mechanism (TOTP, SMS, email)
- **Rate-limit thresholds** — "request throttling" but no attempt count, time window, or lockout duration
- **Session timeout value** — "configurable timeout periods" but no default
- **Remember Me scope** — "persistent login sessions" but no cookie/session duration
- **Theme persistence** — Light/Dark mode options exist, but persistence is not stated

---

## Usage

- **Read the PRD without re-extraction:** the text is already saved in `requirements/PRD_VWO_Login_Text.txt`.
- **Regenerate the rate-limiting test cases:** run the rate-limiting requirement prompt with and without `ch_01_anti_hallucination.md` attached, then compare the outputs.
- **Review any AI-generated test case:** verify each expected result against `requirements/PRD_VWO_Login_Text.txt` before accepting it.
