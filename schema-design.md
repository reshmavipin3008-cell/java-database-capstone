# Database Schema Design for Smart Clinic

---

## MySQL Database Design

The relational database handles structured, transactional, and operational core data requiring strong integrity and relational enforcement (ACID compliance).

### Table: admin
Stores portal administrator credentials and access levels.

- **id**: `INT`, Primary Key, Auto Increment, NOT NULL
- **username**: `VARCHAR(50)`, Unique, NOT NULL
- **email**: `VARCHAR(100)`, Unique, NOT NULL
- **password_hash**: `VARCHAR(255)`, NOT NULL
- **created_at**: `DATETIME`, Default CURRENT_TIMESTAMP

---

### Table: patients
Stores registered patient accounts and basic contact information.

- **id**: `INT`, Primary Key, Auto Increment, NOT NULL
- **first_name**: `VARCHAR(50)`, NOT NULL
- **last_name**: `VARCHAR(50)`, NOT NULL
- **email**: `VARCHAR(100)`, Unique, NOT NULL
- **phone**: `VARCHAR(20)`, NOT NULL
- **password_hash**: `VARCHAR(255)`, NOT NULL
- **date_of_birth**: `DATE`, NOT NULL
- **created_at**: `DATETIME`, Default CURRENT_TIMESTAMP

---

### Table: doctors
Stores profiles, clinical specialties, and contact information for healthcare providers.

- **id**: `INT`, Primary Key, Auto Increment, NOT NULL
- **first_name**: `VARCHAR(50)`, NOT NULL
- **last_name**: `VARCHAR(50)`, NOT NULL
- **specialization**: `VARCHAR(100)`, NOT NULL
- **email**: `VARCHAR(100)`, Unique, NOT NULL
- **phone**: `VARCHAR(20)`, NOT NULL
- **license_number**: `VARCHAR(50)`, Unique, NOT NULL
- **status**: `TINYINT(1)`, Default 1 (1 = Active, 0 = Inactive)
- **created_at**: `DATETIME`, Default CURRENT_TIMESTAMP

---

### Table: doctor_schedules
Manages doctor working hours and available time slots.

- **id**: `INT`, Primary Key, Auto Increment, NOT NULL
- **doctor_id**: `INT`, Foreign Key → `doctors(id)` ON DELETE CASCADE
- **day_of_week**: `TINYINT`, NOT NULL (1 = Monday, 7 = Sunday)
- **start_time**: `TIME`, NOT NULL
- **end_time**: `TIME`, NOT NULL
- **is_available**: `BOOLEAN`, Default TRUE

---

### Table: appointments
Tracks scheduled, completed, and cancelled consultation slots between patients and doctors.

- **id**: `INT`, Primary Key, Auto Increment, NOT NULL
- **patient_id**: `INT`, Foreign Key → `patients(id)` ON DELETE RESTRICT
- **doctor_id**: `INT`, Foreign Key → `doctors(id)` ON DELETE RESTRICT
- **appointment_time**: `DATETIME`, NOT NULL
- **duration_minutes**: `INT`, Default 60
- **status**: `INT`, Default 0 (0 = Scheduled, 1 = Completed, 2 = Cancelled)
- **created_at**: `DATETIME`, Default CURRENT_TIMESTAMP

> **Relational Integrity Justification:**
> - `ON DELETE RESTRICT` is applied to `patient_id` and `doctor_id` in `appointments` to prevent loss of historical clinical/billing records if a patient or doctor profile is removed. Doctors are soft-deleted via `status = 0` instead of dropping DB records.
> - Overlapping appointments are prevented at the application/database constraint level by validating `doctor_id` + `appointment_time` combinations.

---

## MongoDB Collection Design

MongoDB is utilized for flexible, non-relational, and semi-structured clinical documents such as prescriptions, medical notes, and pharmacy metadata.

### Collection: prescriptions

Stores prescription details, free-form physician notes, medication lists, and linked pharmacy information for completed consultations.

```json
{
  "_id": { "$oid": "66a3ef8109d4c21b9c8d1001" },
  "appointmentId": 105,
  "patientId": 42,
  "patientName": "John Smith",
  "doctorId": 12,
  "doctorName": "Dr. Sarah Jenkins",
  "issuedDate": "2026-07-25T08:30:00Z",
  "medications": [
    {
      "name": "Amoxicillin",
      "dosage": "500mg",
      "frequency": "Three times daily",
      "duration": "7 days",
      "refillsRemaining": 1
    },
    {
      "name": "Ibuprofen",
      "dosage": "400mg",
      "frequency": "As needed for pain",
      "duration": "5 days",
      "refillsRemaining": 0
    }
  ],
  "doctorNotes": "Patient presents with mild respiratory symptoms. Drink plenty of fluids and rest. Follow up in one week if fever persists.",
  "pharmacy": {
    "pharmacyId": "PHARM-9921",
    "name": "CVS Pharmacy",
    "address": "123 Main Street, Suite A",
    "phone": "555-0199"
  },
  "tags": ["antibiotic", "respiratory", "routine-checkup"],
  "metadata": {
    "signedDigitally": true,
    "version": 1.0
  }
}
