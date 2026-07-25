# User Story: Patient Logout

**Title:**
_As a patient, I want to log out of the portal, so that I can secure my account when I am finished._

**Acceptance Criteria:**
1. Visible "Log Out" button present in the account navigation menu.
2. Clicking "Log Out" terminates active session and clears auth cookies/tokens.
3. Redirects user back to the home page or login screen.

**Priority:** Medium
**Story Points:** 1
**Notes:**
- Invalidate session on backend server upon request.
