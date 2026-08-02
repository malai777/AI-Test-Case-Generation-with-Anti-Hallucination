# Rate Limiting — Test Cases (Without Anti-Hallucination Rule)

> **Source:** Product Requirements Document: VWO Login Dashboard (app.vwo.com)
> **Requirement:** *Rate Limiting: Protection against brute-force attacks through request throttling.*
> **Note:** Generated WITHOUT the anti-hallucination rule. Values shown in preconditions, test data, steps and expected results are illustrative assumptions introduced by the model — they are not confirmed by the PRD.

---

| ID | Title | Preconditions | Test Data | Test Steps | Expected Result |
|----|-------|---------------|-----------|------------|-----------------|
| UA-01 | Verify rate limit threshold after repeated failed attempts | Valid registered user account exists | 5 failed login attempts | 1. Submit 5 consecutive failed login attempts 2. Attempt a 6th login | 1. Login is blocked/throttled after the 5th failed attempt |
| UA-02 | Verify account lockout duration | User locked out after threshold | 5 failed attempts, 15-minute wait | 1. Trigger lockout with 5 failed attempts 2. Attempt login again immediately 3. Wait 15 minutes and retry | 1. Login is rejected immediately after lockout 2. Account is unlocked after 15 minutes and login succeeds |
| UA-03 | Verify warning before lockout | User has made 4 failed attempts | 4 failed attempts | 1. Make 4 failed attempts 2. Attempt the 5th login | 1. User is warned that only one attempt remains before lockout |
| UA-04 | Verify rate limiting is applied per IP address | Test IP address; no previous attempts | 5 failed attempts from same IP | 1. Submit 5 failed attempts from IP X 2. Attempt login from same IP | 1. Requests from IP X are throttled after 5 failures |
| UA-05 | Verify different IP addresses are unaffected | Two test IP addresses | 5 failed attempts from IP X | 1. Lock out IP X with 5 failed attempts 2. Attempt login from IP Y | 1. IP Y can log in normally; only IP X is throttled |
| UA-06 | Verify exponential backoff on repeated failures | None | Consecutive failed attempts | 1. Continue failed attempts after initial throttling | 1. Delay between allowed attempts increases progressively (e.g., 1 minute, then 5 minutes, then 15 minutes) |
| UA-07 | Verify password-reset rate limiting | Valid registered email | 3 reset requests in 1 hour | 1. Request password reset 3 times within an hour 2. Request a 4th time | 1. Password-reset requests are limited to 3 per hour |
| UA-08 | Verify CAPTCHA after repeated failures | None | 5 failed attempts | 1. Make 5 failed attempts 2. Attempt the 6th login | 1. A CAPTCHA challenge is displayed before the next login attempt |
| UA-09 | Verify login API returns 429 on throttling | Login API accessible; threshold reached | 6th attempt after 5 failures | 1. Submit login request to `/api/v1/login` after exceeding threshold | 1. API returns HTTP status 429 Too Many Requests |
| UA-10 | Verify `Retry-After` header on throttled response | Login API accessible; threshold reached | Throttled request | 1. Inspect the response headers of a throttled login request | 1. Response includes a `Retry-After` header indicating wait time |
| UA-11 | Verify failed-attempt counter resets after successful login | Valid credentials | 4 failed attempts then valid login | 1. Make 4 failed attempts 2. Log in successfully 3. Make 4 more failed attempts | 1. Counter resets on successful login; user can make 5 fresh attempts before lockout |
| UA-12 | Verify admin can configure threshold and duration | Admin access to security settings | Threshold = 10, duration = 30 min | 1. Admin changes the failure threshold and lockout duration 2. Trigger a lockout | 1. The new threshold and duration are enforced as configured |
| UA-13 | Verify security logs record throttling events | Logging enabled; threshold reached | Throttled request | 1. Trigger a throttling event 2. Inspect the security log | 1. Log entry contains timestamp, source IP and account details |
| UA-14 | Verify throttling enforced per account and per IP | Valid account; two IPs | 5 failed attempts per scope | 1. Exceed threshold for account A from IP X 2. Attempt login to account A from IP Y | 1. Throttling is enforced independently for the account and the IP |
| UA-15 | Verify legitimate user is not affected below threshold | Valid credentials | 3 failed attempts then valid login | 1. Make 3 failed attempts 2. Log in with valid credentials | 1. Valid login succeeds below the threshold; no throttling applied |

---

## Notes (from model)

> The threshold and duration values used above are illustrative examples and should be confirmed with the security team before test execution.
