# User Story: Run Monthly Appointment Usage Statistics

**Title:**
_As an admin, I want to run a stored procedure in MySQL CLI to get the number of appointments per month, so that I can track platform usage statistics._

**Acceptance Criteria:**
1. MySQL stored procedure (`GetMonthlyAppointmentStats`) created to aggregate appointment counts grouped by month and year.
2. Procedure can be executed via MySQL CLI by an authenticated DB admin.
3. Results output clearly formatted columns showing Year, Month, Total Appointments, and Status breakdowns.

**Priority:** Medium
**Story Points:** 5
**Notes:**
- Optimize procedure query with proper indexing on appointment timestamp columns for fast execution.
