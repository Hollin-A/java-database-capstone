# Schema Design for Smart Clinic Management System

This document outlines the database design for the Smart Clinic Management System, utilizing both MySQL for structured relational data and MongoDB for flexible document-based data.

## MySQL Database Design

The MySQL database will handle the core operational data that requires strict relationships, validation, and ACID compliance.

### Table: patients
- id: INT, Primary Key, Auto Increment
- first_name: VARCHAR(50), Not Null
- last_name: VARCHAR(50), Not Null
- email: VARCHAR(100), Unique, Not Null
- phone: VARCHAR(15), Not Null
- date_of_birth: DATE, Not Null
- gender: ENUM('Male', 'Female', 'Other'), Not Null
- address: TEXT
- emergency_contact_name: VARCHAR(100)
- emergency_contact_phone: VARCHAR(15)
- insurance_provider: VARCHAR(100)
- insurance_policy_number: VARCHAR(50)
- created_at: TIMESTAMP, Default CURRENT_TIMESTAMP
- updated_at: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

**Design Notes:**
- Email uniqueness ensures no duplicate patient accounts
- Emergency contact information is crucial for medical facilities
- Insurance information helps with billing and coverage verification

### Table: doctors
- id: INT, Primary Key, Auto Increment
- first_name: VARCHAR(50), Not Null
- last_name: VARCHAR(50), Not Null
- email: VARCHAR(100), Unique, Not Null
- phone: VARCHAR(15), Not Null
- specialization: VARCHAR(100), Not Null
- license_number: VARCHAR(50), Unique, Not Null
- years_of_experience: INT, Default 0
- education: TEXT
- bio: TEXT
- consultation_fee: DECIMAL(10,2), Default 0.00
- is_active: BOOLEAN, Default TRUE
- clinic_location_id: INT, Foreign Key → clinic_locations(id)
- created_at: TIMESTAMP, Default CURRENT_TIMESTAMP
- updated_at: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

**Design Notes:**
- License number uniqueness ensures regulatory compliance
- is_active flag allows soft deletion (keeping appointment history)
- Consultation fee stored for billing purposes

### Table: appointments
- id: INT, Primary Key, Auto Increment
- doctor_id: INT, Foreign Key → doctors(id), Not Null
- patient_id: INT, Foreign Key → patients(id), Not Null
- appointment_date: DATE, Not Null
- appointment_time: TIME, Not Null
- duration_minutes: INT, Default 60
- status: ENUM('Scheduled', 'Confirmed', 'In_Progress', 'Completed', 'Cancelled', 'No_Show'), Default 'Scheduled'
- appointment_type: ENUM('Consultation', 'Follow_up', 'Emergency', 'Routine_Checkup'), Default 'Consultation'
- notes: TEXT
- created_at: TIMESTAMP, Default CURRENT_TIMESTAMP
- updated_at: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

**Design Notes:**
- Separate date and time fields for easier querying and scheduling
- Status enum covers all possible appointment states
- Duration allows for flexible appointment lengths
- Constraint: No overlapping appointments for same doctor

### Table: admin
- id: INT, Primary Key, Auto Increment
- username: VARCHAR(50), Unique, Not Null
- email: VARCHAR(100), Unique, Not Null
- password_hash: VARCHAR(255), Not Null
- first_name: VARCHAR(50), Not Null
- last_name: VARCHAR(50), Not Null
- role: ENUM('Super_Admin', 'Admin', 'Manager'), Default 'Admin'
- is_active: BOOLEAN, Default TRUE
- last_login: TIMESTAMP
- created_at: TIMESTAMP, Default CURRENT_TIMESTAMP
- updated_at: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

**Design Notes:**
- Password stored as hash for security
- Role-based access control for different admin levels
- last_login tracking for security monitoring

### Table: clinic_locations
- id: INT, Primary Key, Auto Increment
- name: VARCHAR(100), Not Null
- address: TEXT, Not Null
- city: VARCHAR(50), Not Null
- state: VARCHAR(50), Not Null
- zip_code: VARCHAR(10), Not Null
- phone: VARCHAR(15), Not Null
- email: VARCHAR(100)
- operating_hours_start: TIME, Default '08:00:00'
- operating_hours_end: TIME, Default '18:00:00'
- is_active: BOOLEAN, Default TRUE
- created_at: TIMESTAMP, Default CURRENT_TIMESTAMP

**Design Notes:**
- Supports multi-location clinics
- Operating hours help with appointment scheduling
- Separate contact information per location

### Table: doctor_availability
- id: INT, Primary Key, Auto Increment
- doctor_id: INT, Foreign Key → doctors(id), Not Null
- day_of_week: ENUM('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'), Not Null
- start_time: TIME, Not Null
- end_time: TIME, Not Null
- is_available: BOOLEAN, Default TRUE
- created_at: TIMESTAMP, Default CURRENT_TIMESTAMP

**Design Notes:**
- Defines regular weekly availability for each doctor
- Allows for different schedules per day
- Can be overridden by specific unavailability records

### Table: payments
- id: INT, Primary Key, Auto Increment
- appointment_id: INT, Foreign Key → appointments(id), Not Null
- amount: DECIMAL(10,2), Not Null
- payment_method: ENUM('Cash', 'Credit_Card', 'Debit_Card', 'Insurance', 'Online'), Not Null
- payment_status: ENUM('Pending', 'Completed', 'Failed', 'Refunded'), Default 'Pending'
- transaction_id: VARCHAR(100), Unique
- payment_date: TIMESTAMP, Default CURRENT_TIMESTAMP
- notes: TEXT

**Design Notes:**
- Links payments to specific appointments
- Supports multiple payment methods
- Transaction ID for external payment gateway integration

## MongoDB Collection Design

MongoDB will handle flexible, document-based data that doesn't fit well into rigid table structures.

### Collection: prescriptions
```json
{
  "_id": ObjectId("64abc123456789012345678"),
  "appointmentId": 101,
  "patientId": 25,
  "doctorId": 12,
  "patientName": "John Smith",
  "doctorName": "Dr. Sarah Johnson",
  "prescriptionDate": "2024-02-15T10:30:00Z",
  "medications": [
    {
      "name": "Paracetamol",
      "genericName": "Acetaminophen",
      "dosage": "500mg",
      "frequency": "Every 6 hours",
      "duration": "5 days",
      "instructions": "Take with food to avoid stomach upset",
      "quantity": 20,
      "refillsAllowed": 2
    },
    {
      "name": "Ibuprofen",
      "genericName": "Ibuprofen",
      "dosage": "400mg",
      "frequency": "Twice daily",
      "duration": "7 days",
      "instructions": "Take after meals",
      "quantity": 14,
      "refillsAllowed": 0
    }
  ],
  "diagnosis": "Mild fever and headache",
  "doctorNotes": "Patient responded well to treatment. Follow up if symptoms persist beyond prescribed duration.",
  "pharmacy": {
    "name": "Walgreens Pharmacy",
    "address": "123 Market Street, San Francisco, CA",
    "phone": "+1-555-0123"
  },
  "allergies": ["Penicillin", "Shellfish"],
  "warnings": [
    "Do not exceed recommended dosage",
    "Avoid alcohol while taking medication"
  ],
  "isActive": true,
  "createdAt": "2024-02-15T10:30:00Z",
  "updatedAt": "2024-02-15T10:30:00Z"
}
```

**Design Notes:**
- Flexible structure allows for varying numbers of medications
- Embedded pharmacy information for convenience
- Array fields for allergies and warnings
- Rich metadata that would be difficult to model in SQL

### Collection: patient_feedback
```json
{
  "_id": ObjectId("64def987654321098765432"),
  "appointmentId": 101,
  "patientId": 25,
  "doctorId": 12,
  "rating": {
    "overall": 4.5,
    "communication": 5,
    "punctuality": 4,
    "professionalism": 5,
    "facilityQuality": 4
  },
  "comments": "Dr. Johnson was very thorough and explained everything clearly. The wait time was a bit longer than expected, but the quality of care was excellent.",
  "wouldRecommend": true,
  "feedbackDate": "2024-02-15T14:30:00Z",
  "isAnonymous": false,
  "tags": ["thorough", "professional", "clear-communication"],
  "category": "General Consultation",
  "responseFromDoctor": {
    "message": "Thank you for your feedback. I'm glad I could help with your concerns.",
    "respondedAt": "2024-02-16T09:00:00Z"
  },
  "isPublic": true,
  "createdAt": "2024-02-15T14:30:00Z"
}
```

**Design Notes:**
- Flexible rating system with multiple criteria
- Free-form comments field
- Tag system for categorizing feedback
- Optional doctor responses
- Privacy controls (anonymous, public flags)

### Collection: appointment_notes
```json
{
  "_id": ObjectId("64ghi234567890123456789"),
  "appointmentId": 101,
  "doctorId": 12,
  "patientId": 25,
  "chiefComplaint": "Persistent headache and mild fever for 3 days",
  "symptoms": [
    {
      "symptom": "Headache",
      "severity": "Moderate",
      "duration": "3 days",
      "location": "Frontal"
    },
    {
      "symptom": "Fever",
      "severity": "Mild",
      "duration": "2 days",
      "temperature": "100.2°F"
    }
  ],
  "vitalSigns": {
    "bloodPressure": "120/80",
    "heartRate": 75,
    "temperature": "100.2°F",
    "weight": "165 lbs",
    "height": "5'8\"",
    "oxygenSaturation": "98%"
  },
  "physicalExamination": {
    "general": "Patient appears well, alert and oriented",
    "head": "Normocephalic, no signs of trauma",
    "neck": "Supple, no lymphadenopathy",
    "cardiovascular": "Regular rate and rhythm, no murmurs",
    "respiratory": "Clear to auscultation bilaterally"
  },
  "diagnosis": {
    "primary": "Viral syndrome with cephalgia",
    "differential": ["Tension headache", "Sinusitis"],
    "icdCode": "R50.9"
  },
  "treatmentPlan": [
    "Symptomatic treatment with OTC analgesics",
    "Adequate rest and hydration",
    "Follow-up if symptoms worsen or persist beyond 7 days"
  ],
  "followUpRequired": true,
  "followUpDate": "2024-02-22",
  "attachments": [
    {
      "type": "lab_result",
      "filename": "blood_work_20240215.pdf",
      "uploadedAt": "2024-02-15T11:00:00Z"
    }
  ],
  "createdAt": "2024-02-15T11:30:00Z",
  "updatedAt": "2024-02-15T11:30:00Z"
}
```

**Design Notes:**
- Comprehensive medical documentation structure
- Flexible symptom and examination recording
- Support for file attachments
- Structured diagnosis with ICD coding
- Rich metadata that would be complex to model in relational tables

### Collection: system_logs
```json
{
  "_id": ObjectId("64jkl345678901234567890"),
  "timestamp": "2024-02-15T10:30:00Z",
  "logLevel": "INFO",
  "userId": 25,
  "userType": "patient",
  "action": "appointment_booked",
  "resourceType": "appointment",
  "resourceId": 101,
  "details": {
    "doctorId": 12,
    "appointmentDate": "2024-02-20",
    "appointmentTime": "14:00",
    "duration": 60
  },
  "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
  "ipAddress": "192.168.1.100",
  "sessionId": "sess_abc123def456",
  "metadata": {
    "source": "web_portal",
    "version": "1.2.3",
    "environment": "production"
  }
}
```

**Design Notes:**
- Comprehensive activity logging for audit trails
- Flexible metadata structure for different log types
- User tracking and session management
- Security monitoring capabilities
- Easy to query for analytics and troubleshooting

## Design Rationale

### Why MySQL for Core Data?
- **ACID Compliance**: Critical for appointment scheduling and patient data integrity
- **Referential Integrity**: Foreign key relationships ensure data consistency
- **Complex Queries**: SQL joins are perfect for appointment-patient-doctor relationships
- **Regulatory Compliance**: Healthcare data requires strict validation and audit trails

### Why MongoDB for Flexible Data?
- **Schema Evolution**: Medical notes and feedback structures can change over time
- **Rich Documents**: Prescriptions with multiple medications fit naturally as documents
- **Performance**: Fast reads for displaying patient history and notes
- **Flexibility**: Easy to add new fields without schema migrations

### Data Flow Strategy
1. **Core Operations** (scheduling, user management) → MySQL
2. **Rich Content** (notes, feedback, logs) → MongoDB
3. **References**: MongoDB documents reference MySQL IDs for consistency
4. **Backup Strategy**: Both databases backed up regularly with point-in-time recovery

This hybrid approach leverages the strengths of both database types while maintaining data integrity and system performance.