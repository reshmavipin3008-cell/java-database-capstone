# User Story: Book Hour-Long Appointment

**Title:**
_As a logged-in patient, I want to book an hour-long appointment, so that I can consult with a doctor._

**Acceptance Criteria:**
1. Interactive scheduling calendar showing available 1-hour time slots for selected doctors.
2. Ability to select date, slot, and submit booking details.
3. System confirms booking, prevents double-booking for the selected time slot, and sends confirmation notification.

**Priority:** High
**Story Points:** 5
**Notes:**
- System must automatically lock selected 1-hour slot during checkout/booking process to prevent race conditions.
