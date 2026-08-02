# VWO Login Dashboard — Missing Facts Analysis

> **Source:** Product Requirements Document: VWO Login Dashboard (app.vwo.com)
> **Extracted text:** `requirements/PRD_VWO_Login_Text.txt`
> **Reviewed test cases:** `generated-testcases/vwo_login_testcases.md`
> **Purpose:** Cross-reference every generated test case against the PRD to identify facts the PRD does not state, which caused the AI to hallucinate or over-assume behavior in test cases.

---

## Missing Facts vs. Affected Test Cases

### 1. Password complexity rules (undefined)
- **PRD says:** "Enforced security standards for password complexity"
- **Missing fact:** Minimum length, required character classes (upper/lower/digit/special), max length, no-username rule, etc.
- **Affected TCs:** `VAL-06`, `VAL-07`, `VAL-08`, `PM-06`, `PM-07`
- **Impact:** TCs invent "weak/short password" vs "complex password" definitions that do not exist in the PRD. Expected results ("indicator reflects low strength", "password is rejected") are unverifiable without concrete rules.

### 2. 2FA method / mechanism (undefined)
- **PRD says:** "Optional 2FA support for enhanced security"
- **Missing fact:** Which factor (TOTP app, SMS, email OTP), how the code is delivered, expiry window, retry limits.
- **Affected TCs:** `AUTH-08`, `AUTH-09`, `AUTH-10`
- **Impact:** TCs assume a "2FA code" flow and fabricate invalid/expired-code rejection behavior not stated in the PRD.

### 3. Required-field behavior on empty submit (undefined)
- **PRD says:** nothing about fields being required or empty-submit behavior.
- **Missing fact:** Whether fields are required, and what happens on submit with empty fields.
- **Affected TCs:** `UI-06`
- **Impact:** TC asserts "validation errors are shown for both fields" — invented behavior.

### 4. Theme persistence (undefined)
- **PRD says:** Light and Dark Mode options exist (in announcement banner).
- **Missing fact:** Whether the selected theme persists across sessions, and default theme.
- **Affected TCs:** `UI-15`
- **Impact:** TC asserts "selected theme is applied consistently / persists" — persistence is not stated.

### 5. Session timeout default value (undefined)
- **PRD says:** "configurable timeout periods"
- **Missing fact:** Default timeout value, allowed range, idle vs absolute expiry semantics.
- **Affected TCs:** `AUTH-06`, `AUTH-07`
- **Impact:** TCs only testable against an abstract "T" with no concrete value.

### 6. Remember Me semantics (undefined)
- **PRD says:** "Checkbox option for persistent login sessions"
- **Missing fact:** Cookie duration, whether browser close clears the session, device scope.
- **Affected TCs:** `UI-08`, `UI-09`
- **Impact:** TCs infer browser-close behavior ("reopen browser → still logged in / must log in again") not stated in the PRD.

### 7. Non-email recovery options (undefined)
- **PRD says:** "Multiple recovery options including email-based reset"
- **Missing fact:** What the other recovery options are (SMS, security questions, support contact, etc.).
- **Affected TCs:** `PM-04`, `PM-05`
- **Impact:** `PM-05` asserts multiple options exist beyond email, but only email is named; the others are unverifiable.

### 8. Error message copy / content (undefined)
- **PRD says:** "Clear, actionable error messages for failed authentication attempts"
- **Missing fact:** Actual message text, per-failure-type wording (wrong password vs unregistered email), localization.
- **Affected TCs:** `AUTH-02`, `AUTH-03`, `VAL-09`, `VAL-10`, `JNY-07`
- **Impact:** TCs can only assert "an error message appears"; content/actionability cannot be verified against the PRD.

### 9. Rate-limiting thresholds (undefined)
- **PRD says:** "Protection against brute force attacks through request throttling"
- **Missing fact:** Max attempts, lockout time window, IP vs account-based throttling, CAPTCHA triggers.
- **Affected TCs:** `SEC-05`, `SEC-06`
- **Impact:** "Multiple rapid failed attempts" / "sustained attack pattern" have no defined threshold to assert against.

### 10. Social login cancellation behavior (undefined)
- **PRD says:** "Optional integration with Google, Microsoft, and other identity providers"
- **Missing fact:** What happens when the user cancels/denies the IdP authorization.
- **Affected TCs:** `AUTH-18`
- **Impact:** TC asserts "user returns to the login page without an erroneous session" — invented behavior.

### 11. High-contrast mode enablement (undefined)
- **PRD says:** "Accessibility options for visually impaired users"
- **Missing fact:** Whether high-contrast is an in-page toggle or follows OS/browser settings.
- **Affected TCs:** `ACC-03`, `ACC-04`
- **Impact:** TC steps "enable the high contrast accessibility option" assume an in-page control that is not stated.

### 12. SSO entry point / IdP specifics (undefined)
- **PRD says:** "Enterprise SSO integration capabilities", "Support for SAML, OAuth, and other enterprise authentication protocols"
- **Missing fact:** Where SSO is initiated from (login-page button, separate URL), IdP discovery, redirect behavior.
- **Affected TCs:** `AUTH-11` to `AUTH-15`, `INT-04` to `INT-06`
- **Impact:** TCs assume an SSO option exists on the login page and that SAML/OAuth flows are user-initiable from it.

### 13. Existing-UI specifics (no screenshots provided)
- **PRD says:** Current features are described as "based on analysis of the existing VWO login interface"; UI text and layout are asserted (banner, branding, form).
- **Missing fact:** Screenshots of the actual login page (`screenshots/` folder is empty).
- **Affected TCs:** `UI-01` to `UI-05`, `UI-10` to `UI-16`, `UX-01` to `UX-05`
- **Impact:** Exact labels, button text, banner copy, theme-toggle labels, and visual layout cannot be verified. Any TC asserting exact UI text is unverifiable without screenshots.

---

## Suggested Fixes

1. **Clarify the PRD** — add concrete values for password complexity, timeout, rate-limit thresholds, Remember Me duration, and 2FA method, or mark them as "to be defined" so TCs treat them as placeholders.
2. **Add screenshots** of the actual login page to `screenshots/` to ground UI-level assertions.
3. **Rewrite affected TCs** to assert only what the PRD states; label inferred behavior as "Inference (low confidence)" per the anti-hallucination rules, or drop the invented assertions.
