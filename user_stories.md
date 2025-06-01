# User Stories - Healthcare Portal

## Exercise 2: Admin User Stories

### US-001: Admin Login
**Title:**
*As an admin, I want to log into the portal with my username and password, so that I can manage the platform securely.*

**Acceptance Criteria:**
1. Admin can enter valid username and password on login form
2. System validates credentials against admin database
3. Upon successful login, admin is redirected to admin dashboard
4. Failed login attempts display appropriate error message
5. Login session expires after defined period of inactivity

**Priority:** High
**Story Points:** 3
**Notes:**
- Implement rate limiting for failed login attempts
- Consider two-factor authentication for enhanced security
- Log all admin login attempts for audit purposes

### US-002: Admin Logout
**Title:**
*As an admin, I want to log out of the portal, so that I can protect system access when I'm done working.*

**Acceptance Criteria:**
1. Admin can click logout button from any admin page
2. System immediately terminates admin session
3. Admin is redirected to login page after logout
4. All cached admin data is cleared from browser
5. Attempting to access admin pages after logout redirects to login

**Priority:** High
**Story Points:** 2
**Notes:**
- Implement automatic logout after session timeout
- Clear all authentication tokens on logout

### US-003: Add Doctor Profile
**Title:**
*As an admin, I want to add doctors to the portal, so that patients can find and book appointments with qualified medical professionals.*

**Acceptance Criteria:**
1. Admin can access "Add Doctor" form from admin dashboard
2. Form includes required fields: name, email, specialization, contact info, credentials
3. System validates all required fields before submission
4. New doctor profile is created and stored in database
5. Confirmation message displayed upon successful doctor addition
6. New doctor receives welcome email with login credentials

**Priority:** High
**Story Points:** 5
**Notes:**
- Validate medical license numbers
- Support uploading doctor profile photos
- Send automated email to new doctors with portal access instructions

### US-004: Delete Doctor Profile
**Title:**
*As an admin, I want to delete a doctor's profile from the portal, so that I can remove inactive or inappropriate medical professionals.*

**Acceptance Criteria:**
1. Admin can search and select doctor profiles to delete
2. System displays confirmation dialog before deletion
3. All associated appointments are handled (cancelled/transferred)
4. Doctor profile and related data are permanently removed
5. Deletion action is logged for audit purposes
6. Affected patients are notified of appointment cancellations

**Priority:** Medium
**Story Points:** 4
**Notes:**
- Consider soft delete option for data retention
- Handle existing appointments before deletion
- Maintain deletion logs for compliance

### US-005: Generate Appointment Statistics
**Title:**
*As an admin, I want to run a stored procedure in MySQL CLI to get the number of appointments per month, so that I can track usage statistics and platform performance.*

**Acceptance Criteria:**
1. Stored procedure calculates monthly appointment counts
2. Results include current and previous months for comparison
3. Data can be exported to CSV format
4. Procedure handles date range parameters
5. Results display total appointments, completed, and cancelled counts

**Priority:** Low
**Story Points:** 3
**Notes:**
- Include additional metrics like doctor utilization rates
- Consider creating dashboard view for real-time statistics
- Implement automated monthly reporting

## Exercise 3: Patient User Stories

### US-006: Browse Doctors Without Login
**Title:**
*As a patient, I want to view a list of doctors without logging in, so that I can explore available options before registering.*

**Acceptance Criteria:**
1. Public page displays list of all active doctors
2. Doctor cards show basic info: name, specialization, rating, availability
3. Patients can filter doctors by specialization
4. Search functionality allows finding doctors by name
5. "Book Appointment" buttons redirect to registration/login

**Priority:** High
**Story Points:** 4
**Notes:**
- Implement doctor rating and review system
- Add map integration for location-based search
- Optimize page loading for better user experience

### US-007: Patient Registration
**Title:**
*As a patient, I want to sign up using my email and password, so that I can book appointments with doctors.*

**Acceptance Criteria:**
1. Registration form includes email, password, name, phone, date of birth
2. Email validation ensures unique and valid email addresses
3. Password requirements include minimum length and complexity
4. Account verification email sent upon registration
5. New patient profile created after email verification

**Priority:** High
**Story Points:** 4
**Notes:**
- Implement email verification process
- Add CAPTCHA to prevent automated registrations
- Store medical history and insurance information optionally

### US-008: Patient Login
**Title:**
*As a patient, I want to log into the portal, so that I can manage my bookings and access my appointment history.*

**Acceptance Criteria:**
1. Login form accepts email and password
2. "Remember me" option for convenient future logins
3. "Forgot password" link for password recovery
4. Successful login redirects to patient dashboard
5. Invalid credentials show appropriate error messages

**Priority:** High
**Story Points:** 3
**Notes:**
- Implement password recovery via email
- Add social login options (Google, Facebook)
- Track login history for security purposes

### US-009: Patient Logout
**Title:**
*As a patient, I want to log out of the portal, so that I can secure my account when using shared devices.*

**Acceptance Criteria:**
1. Logout option visible on all patient pages
2. Immediate session termination upon logout
3. Redirect to homepage after logout
4. Clear all cached patient data
5. Show logout confirmation message

**Priority:** Medium
**Story Points:** 2
**Notes:**
- Implement automatic logout for security
- Clear sensitive data from browser storage

### US-010: Book Appointment
**Title:**
*As a patient, I want to log in and book an hour-long appointment, so that I can consult with a doctor for my healthcare needs.*

**Acceptance Criteria:**
1. Patient can select doctor from available list
2. Calendar shows doctor's available time slots
3. Appointment duration is fixed at one hour
4. Booking confirmation sent via email and SMS
5. Appointment details saved to patient's profile
6. Payment processing for appointment fees

**Priority:** High
**Story Points:** 6
**Notes:**
- Integrate payment gateway for appointment fees
- Send calendar invites to patients
- Implement appointment reminder system

### US-011: View Upcoming Appointments
**Title:**
*As a patient, I want to view my upcoming appointments, so that I can prepare accordingly and manage my schedule.*

**Acceptance Criteria:**
1. Dashboard displays chronological list of upcoming appointments
2. Each appointment shows date, time, doctor name, and specialization
3. Options to reschedule or cancel appointments
4. Appointment reminders and preparation instructions
5. Links to join virtual consultations if applicable

**Priority:** Medium
**Story Points:** 3
**Notes:**
- Add appointment preparation checklists
- Include doctor's office location and directions
- Show appointment status (confirmed, pending, cancelled)

## Exercise 4: Doctor User Stories

### US-012: Doctor Login
**Title:**
*As a doctor, I want to log into the portal, so that I can manage my appointments and patient information securely.*

**Acceptance Criteria:**
1. Login form accepts doctor credentials (email/username and password)
2. Two-factor authentication for enhanced security
3. Successful login redirects to doctor dashboard
4. Failed login attempts are logged and limited
5. Session timeout for inactive periods

**Priority:** High
**Story Points:** 3
**Notes:**
- Implement strong authentication for medical data protection
- Add biometric login options for mobile devices
- Maintain login audit logs for compliance

### US-013: Doctor Logout
**Title:**
*As a doctor, I want to log out of the portal, so that I can protect my data and patient information from unauthorized access.*

**Acceptance Criteria:**
1. Logout button accessible from all doctor pages
2. Immediate session termination and data clearing
3. Redirect to secure login page
4. All patient data removed from browser cache
5. Logout action logged for security audit

**Priority:** High
**Story Points:** 2
**Notes:**
- Automatic logout after defined inactivity period
- Secure cleanup of all cached medical data
- HIPAA compliance considerations

### US-014: View Appointment Calendar
**Title:**
*As a doctor, I want to view my appointment calendar, so that I can stay organized and manage my daily schedule effectively.*

**Acceptance Criteria:**
1. Calendar displays daily, weekly, and monthly views
2. Appointments show patient name, time, and appointment type
3. Color coding for different appointment types or statuses
4. Integration with external calendar systems (Google, Outlook)
5. Print functionality for physical calendar copies

**Priority:** Medium
**Story Points:** 4
**Notes:**
- Sync with popular calendar applications
- Mobile-responsive design for tablet/phone access
- Add appointment notes and patient history preview

### US-015: Manage Availability
**Title:**
*As a doctor, I want to mark my unavailability, so that I can inform patients of only the available slots and prevent double bookings.*

**Acceptance Criteria:**
1. Calendar interface allows blocking time slots
2. Recurring unavailability patterns (weekly, monthly)
3. Emergency unavailability can be set immediately
4. Blocked slots are hidden from patient booking interface
5. Automatic patient notification for affected appointments

**Priority:** High
**Story Points:** 4
**Notes:**
- Handle existing appointments when marking unavailable
- Allow partial day blocking (lunch breaks, meetings)
- Integrate with vacation and leave management

### US-016: Update Doctor Profile
**Title:**
*As a doctor, I want to update my profile with specialization and contact information, so that patients have up-to-date and accurate information about my services.*

**Acceptance Criteria:**
1. Profile form includes specialization, bio, contact info, education
2. Photo upload functionality for profile picture
3. Validation for medical license and certification numbers
4. Changes require admin approval for critical information
5. Updated information reflects immediately on patient-facing pages

**Priority:** Medium
**Story Points:** 3
**Notes:**
- Version control for profile changes
- Professional photo guidelines and requirements
- Integration with medical licensing verification systems

### US-017: View Patient Details
**Title:**
*As a doctor, I want to view patient details for upcoming appointments, so that I can be prepared and provide personalized healthcare services.*

**Acceptance Criteria:**
1. Patient summary includes medical history, allergies, medications
2. Previous appointment notes and diagnoses
3. Insurance information and coverage details
4. Emergency contact information
5. Secure access with audit logging for HIPAA compliance

**Priority:** High
**Story Points:** 5
**Notes:**
- Implement role-based access controls
- Add patient photo for identification
- Include relevant test results and imaging
- Ensure HIPAA compliance for all patient data access