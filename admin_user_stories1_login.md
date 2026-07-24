# User Story: Admin Portal Login

**Title:**
_As an admin, I want to log into the portal with my username and password, so that I can manage the platform securely._

**Acceptance Criteria:**
1. Secure login form accepting valid admin username and password.
2. System authenticates credentials against the user database and generates a secure session token.
3. Appropriate error message displayed on invalid login attempt without revealing specific credential failure.

**Priority:** High
**Story Points:** 3
**Notes:**
- Implement rate limiting to protect against brute-force attacks.
