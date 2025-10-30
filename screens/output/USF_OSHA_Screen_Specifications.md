# USF OSHA Prerequisites Application - Screen Specifications

## Document Information
- **Application:** USF OSHA Prerequisites Application
- **Source Requirements:** USF_OSHA_Prereqs_Solution_10_28_2025.txt
- **Date Generated:** 2025-10-30
- **Data Model Reference:** USF_OSHA_Data_Model.yaml

## Screen Inventory

### Applicant Screens
1. Applicant Landing Page (Dashboard)
2. Application Wizard - Course Selection
3. Application Wizard - Personal Information
4. Application Wizard - Work Experience
5. Application Wizard - Education & Certifications
6. Application Wizard - Prerequisites & Documents
7. Application Wizard - Review & Sign
8. Application Status View
9. My Applications List

### Approver Screens
10. Approver Dashboard
11. Applications Queue (Record List)
12. Application Review Interface
13. Watch List Verification Screen

### Administrator Screens
14. Administrator Dashboard
15. Watch List Management Interface
16. Course Data Import Interface

### Facilitator/Leadership Screens
17. Approved Applicants Report
18. Application Statistics Dashboard
19. Task Report

---

## Detailed Screen Specifications

---

### Screen 1: Applicant Landing Page

#### Screen Type
Landing Page

#### Target Persona
Applicants

#### Purpose and Business Context
Main entry point for applicants after self-registration and login. Provides overview of their application status, quick actions to start or continue applications, and access to all applicant-facing features. Reduces applicant anxiety by clearly showing application status and next steps.

#### Interface Parameters
N/A (landing page)

#### Data and Fields Displayed

**Welcome Section:**
- Applicant Name: Text, greeting message with user's full name
- Language Toggle: Dropdown (English/Spanish) - switches entire interface language
- Last Login: Date/time of previous login

**Application Status Summary Cards:**
- Number of Applications in Draft: Count with link to draft applications
- Number of Applications Submitted: Count with link to submitted applications
- Number of Applications Under Review: Count with link to applications being reviewed
- Number of Applications Approved: Count with link to approved applications
- Number of Applications Requiring Action: Count with link to applications returned for information

**Quick Actions Section:**
- Start New Application: Primary button launching application wizard
- Continue Draft Application: Secondary button (only shown if drafts exist)
- View All My Applications: Link to full application list

**Recent Activity Feed:**
- Application Number: Linked to application details
- Course Name: Text showing which course(s)
- Status: Badge with color coding (Draft=gray, Submitted=blue, Under Review=yellow, Approved=green, Returned=orange, Denied=red)
- Last Updated: Date/time of most recent status change
- Action Required: Badge indicator if applicant needs to take action

**Upcoming Courses Section:**
- Course Number: Text (e.g., OSHA #500, #501, #5400, #5600)
- Course Name: Full descriptive name
- Start Date: Date
- End Date: Date
- Location: City/State
- Days Until Start: Countdown indicator
- Application Status: Shows if applicant has applied for this course
- Apply Button: Quick action to start application for this specific course

**Help & Resources Section:**
- FAQ Link: Common questions about prerequisites
- Course Requirements Guide: Link to detailed course prerequisite information
- Contact Support: Email link to OSHA training center
- Application Instructions Video: Embedded or linked video tutorial

#### Available Actions and Buttons
- **Start New Application:** Launches application wizard at course selection step
- **Continue Draft Application:** Opens most recent draft application or shows list if multiple drafts exist
- **View All My Applications:** Navigates to My Applications List screen
- **Language Toggle:** Switches between English and Spanish for all applicant-facing screens
- **Apply for Specific Course:** From upcoming courses section, launches wizard with course pre-selected

#### Validation Rules
N/A (informational dashboard)

#### Navigation
- **How users access this screen:** Default landing page after login for Applicant persona; accessible via site navigation "Home" or "Dashboard" link
- **Where users can go from here:** Application Wizard (new or continue), My Applications List, Application Status View (via application number links), Course catalog

#### Related Data and Relationships
- Related to APPLICANT entity (displays applicant information)
- Related to APPLICATION entity (displays application status summary)
- Related to COURSE entity (displays upcoming courses)
- Related to APPLICATION_COURSE entity (links applications to specific courses)

#### Requirements Source
"Applicants will have access to a self-service status page to check on the progress of their application, reducing the need for follow-up emails. The system will also provide automated email confirmations upon successful submission." (Description of To-Be section, line 48)

"All Applicant views will be available in both English and Spanish." (Description of To-Be section, line 48)

#### Priority
High - Primary entry point for applicant persona, critical for user orientation and self-service

---

### Screen 2: Application Wizard - Course Selection

#### Screen Type
Wizard (Step 1 of 6)

#### Target Persona
Applicants

#### Purpose and Business Context
First step of multi-step application wizard. Allows applicants to select one or more courses they wish to apply for. Enforces business rules around contract courses requiring registration codes. Sets the foundation for subsequent wizard steps by determining which prerequisites will be required.

#### Interface Parameters
- **applicationId** (integer) - ID of draft application if continuing existing application, null for new application
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Progress Indicator:**
- Step 1 of 6: Course Selection (current)
- Visual progress bar showing 16% complete

**Instructions Section:**
- Header: "Select Course(s) to Apply For"
- Instruction Text: "You may apply for multiple courses in a single application. Select all courses you wish to attend. Note: Contract courses require a registration code from your employer."
- Deadline Warning: "Applications must be submitted at least 2 weeks before course start date"

**Available Courses Grid:**
For each course, display:
- Course Number: Text (OSHA #500, #501, #5400, #5600, #502, #503, #5402, #5602)
- Course Name: Full descriptive name from COURSE.course_name
- Course Type: Badge (Trainer Course / Update Course)
- Start Date: Date from COURSE.start_date
- End Date: Date from COURSE.end_date
- Location: Text from COURSE.location
- Contract Status: Badge (Open / Contract) from COURSE.is_contract
- Days Until Start: Calculated countdown
- Registration Code Required: Icon indicator if COURSE.is_contract = true
- Select Checkbox: Allows selection of course

**Contract Course Registration Code Section:**
(Only appears when contract course is selected)
- Registration Code: Text input field
- Validation Message: "This code will be validated when you submit your application"
- Help Text: "Contact your employer for the registration code"

**Selected Courses Summary:**
- Count: "You have selected X course(s)"
- List of selected courses with remove option
- Prerequisites Preview: "Based on your selections, you will need to provide: [list of prerequisite types]"

#### Available Actions and Buttons
- **Next:** Proceeds to Step 2 (Personal Information)
  - Validates at least one course is selected
  - If contract course selected, validates registration code is entered (format only, actual validation on submission)
  - Validates selected courses meet 2-week deadline
  - Saves selections and advances wizard
- **Save as Draft:** Saves current selections without validation, allows returning later
- **Cancel:** Returns to dashboard with confirmation dialog if any selections made

#### Validation Rules
- At least one course must be selected to proceed
- Selected course start date must be at least 14 days in the future (2-week cutoff rule)
- If contract course selected, registration code field must be filled (format: alphanumeric, 6-20 characters)
- Cannot select update courses (#502, #503, #5402, #5602) without first verifying prerequisite trainer authorization
- Warning displayed if selecting multiple courses from different industries (construction/general/maritime)

#### Navigation
- **How users access this screen:** "Start New Application" from Applicant Landing Page; "Continue Draft Application" if saved draft exists
- **Where users can go from here:** Next → Step 2 (Personal Information); Cancel → Applicant Landing Page; Save as Draft → Applicant Landing Page with confirmation

#### Related Data and Relationships
- Related to COURSE entity (displays all available courses with COURSE.is_visible = true and COURSE.start_date in future)
- Creates/updates APPLICATION entity (stores draft or in-progress application)
- Creates APPLICATION_COURSE entity records (one per selected course)
- Validates COURSE.registration_code_required and COURSE.is_contract flags

#### Requirements Source
"Applicants will be able to select courses based on the courses available in the Courses synced record. Depending on the course(s) selected, they will have to meet certain prerequisites. Course information fields (number, start date, end date, and location) are all mandatory for all applications." (Application Submission section, line 60)

"'Contract' courses are private offerings for specific institutions, and only their employees can enroll. Applicants for these courses must provide a registration code from their employer, which the system will validate." (Application Submission section, line 62)

"Applicants shall be able to apply for 2 or more courses on a single prerequisite application." (Application Submission section, line 63)

"2 week cutoff for applications prior to course start date" (Notable Business Rules, line 88)

#### Priority
High - Critical first step in primary application workflow

---

### Screen 3: Application Wizard - Personal Information

#### Screen Type
Wizard (Step 2 of 6)

#### Target Persona
Applicants

#### Purpose and Business Context
Second step of wizard collecting applicant personal and contact information required for the OSHA Prerequisite Verification Form. Pre-populates known information from user profile to reduce data entry. Corresponds to "Applicant Information" section of official OSHA form (page 1).

#### Interface Parameters
- **applicationId** (integer) - ID of application being created/edited
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Progress Indicator:**
- Step 2 of 6: Personal Information (current)
- Visual progress bar showing 33% complete
- Breadcrumb: Course Selection > Personal Information

**Instructions Section:**
- Header: "Applicant Information"
- Instruction Text: "Please provide your current contact information. This information will appear on your official OSHA Prerequisite Verification Form."

**Personal Information Section:**
- Applicant Legal Name: Text input, pre-populated from APPLICANT.legal_name (required)
  - Help text: "Must match government-issued ID"
- Job Title: Text input from APPLICATION.job_title (required)
- Company: Text input from APPLICATION.company_name (required)
- Email: Text input, pre-populated from APPLICANT.email (required)
  - Validation: Email format
  - Help text: "All notifications will be sent to this email"
- Phone Number: Text input with format mask (###) ###-#### (required)
  - From APPLICATION.phone_number
- Fax Number: Text input with format mask (###) ###-#### (optional)
  - From APPLICATION.fax_number

**Mailing Address Section:**
- Street Address: Text input from APPLICATION.mailing_address (required)
- City: Text input from APPLICATION.city (required)
- State: Dropdown of US states from APPLICATION.state (required)
- ZIP Code: Text input with 5-digit validation from APPLICATION.zip_code (required)

**Additional Information Section:**
- Preferred Language for Communication: Radio buttons (English / Spanish)
  - From APPLICANT.preferred_language
- Previously Subject to OSHA Revocation/Suspension: Radio buttons (Yes / No) (required)
  - From APPLICATION.has_previous_revocation
  - If Yes: File upload field appears for "OSHA Correspondence Documents"
  - Help text: "If yes, you must attach all OSHA correspondence related to the investigation"

**Data Review Note:**
- Display: "Review your information carefully. This will appear on your official OSHA Prerequisite Verification Form exactly as entered."

#### Available Actions and Buttons
- **Previous:** Returns to Step 1 (Course Selection), saves current data
- **Next:** Proceeds to Step 3 (Work Experience)
  - Validates all required fields
  - Validates email and phone formats
  - If "has previous revocation" = Yes, validates document is uploaded
  - Saves data and advances wizard
- **Save as Draft:** Saves current data without validation, allows returning later
- **Cancel:** Returns to dashboard with confirmation dialog

#### Validation Rules
- All required fields must be filled
- Email must be valid email format
- Phone and fax numbers must match (###) ###-#### format
- ZIP code must be exactly 5 digits
- If "Previously Subject to OSHA Revocation/Suspension" = Yes, at least one document must be uploaded
- Legal name must be at least 2 words (first and last name minimum)
- No special characters allowed in name except hyphens and apostrophes

#### Navigation
- **How users access this screen:** Next from Step 1 (Course Selection); Direct navigation if returning to draft; Previous from Step 3
- **Where users can go from here:** Previous → Step 1; Next → Step 3 (Work Experience); Cancel → Applicant Landing Page; Save as Draft → Applicant Landing Page

#### Related Data and Relationships
- Updates APPLICATION entity with personal information fields
- Related to APPLICANT entity (pre-populates from user profile)
- Creates SUPPORTING_DOCUMENT records if OSHA correspondence uploaded

#### Requirements Source
"Applicants will follow a wizard-like experience in SAIL to provide the information necessary to populate the OSHA Prerequisite Verification form" (Description of To-Be, line 45)

"Applicant Information - Please type or print. (Read instructions on pages 6-8 before completing this form)" (Appendix D - USF Prerequisite Form, lines 252-257)

"I have previously been subject to revocation, suspension, or probation by OSHA ☐ Yes ☐ No. If responded yes to #41, please attach all OSHA correspondence related to the investigation." (Appendix D, lines 371-372)

#### Priority
High - Required data collection step in primary application workflow

---

### Screen 4: Application Wizard - Work Experience

#### Screen Type
Wizard (Step 3 of 6)

#### Target Persona
Applicants

#### Purpose and Business Context
Third step of wizard collecting detailed work experience information required to verify applicants meet the 3-5 year experience requirement for OSHA trainer courses. Allows adding multiple employer entries. Corresponds to pages 2-4 of the official OSHA Prerequisite Verification Form.

#### Interface Parameters
- **applicationId** (integer) - ID of application being created/edited
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Progress Indicator:**
- Step 3 of 6: Work Experience (current)
- Visual progress bar showing 50% complete
- Breadcrumb: Course Selection > Personal Information > Work Experience

**Instructions Section:**
- Header: "Work Experience"
- Instruction Text: "List your work experience with most recent employer first. You must demonstrate 3-5 years of safety-related experience depending on the course(s) you selected. All fields are required for each employer."
- Experience Requirements Display: Based on courses selected in Step 1, shows:
  - "For OSHA #500 or #501: 5 years of construction/general industry safety experience required"
  - "For OSHA #5400: 5 years of maritime industry safety experience required"
  - "For OSHA #5600: 3 years of safety training experience required"
  - "Note: A bachelor's degree in occupational safety/industrial hygiene or professional certification (CSP, CIH, CMC) may substitute for 2 years of experience"

**Experience Calculator Display:**
- Total Years of Experience: Auto-calculated sum
- Years Qualifying for Selected Courses: Auto-calculated based on percentage safety-related
- Visual indicator: Progress bar showing years accumulated toward requirement

**Employer Entry Repeating Section:**
(Can add multiple employers, minimum 1 required)

For each employer:

**Employer Information:**
- Employer Name: Text input (required) - from WORK_EXPERIENCE.employer_name
- Job Title: Text input (required) - from WORK_EXPERIENCE.job_title
- Employment Start Date: Date picker (mm/dd/yyyy) (required) - from WORK_EXPERIENCE.start_date
- Employment End Date: Date picker (mm/dd/yyyy) or "Present" checkbox (required) - from WORK_EXPERIENCE.end_date
- Years/Months in Position: Auto-calculated display from date range

**Company Address:**
- Company: Text input (required) - from WORK_EXPERIENCE.company_address_line1
- Address: Text input (required) - from WORK_EXPERIENCE.company_address_line2
- City: Text input (required) - from WORK_EXPERIENCE.company_city
- State: Dropdown (required) - from WORK_EXPERIENCE.company_state
- ZIP Code: Text input with 5-digit validation (required) - from WORK_EXPERIENCE.company_zip

**Contact Information:**
- Contact Person: Text input (required) - from WORK_EXPERIENCE.contact_person_name
  - Help text: "Name of supervisor or HR representative who can verify your employment"
- Contact Person's Phone: Text input with format (###) ###-#### (required) - from WORK_EXPERIENCE.contact_person_phone
- Contact Person's Email: Text input with email validation (required) - from WORK_EXPERIENCE.contact_person_email

**Safety Experience Details:**
- Percentage Safety-Related: Number input (0-100%) (required) - from WORK_EXPERIENCE.percent_safety_related
  - Help text: "What percentage of this position was devoted to safety-related tasks?"
  - Visual slider with numeric input
  - Warning if below 35% (does not block submission, per business rule about non-transparent cutoff)
- Hours Per Week on Safety Tasks: Number input (required) - from WORK_EXPERIENCE.hours_per_week_safety
- Total Hours Worked Per Week: Number input (required) - from WORK_EXPERIENCE.hours_per_week_total
  - Auto-calculates percentage if both fields filled

**Describe Safety Responsibilities:**
- Multi-line text area (required) - from WORK_EXPERIENCE.safety_responsibilities
- Character limit: 1000 characters
- Character counter display
- Help text: "List safety-related tasks performed on the job, including responsibility for safety of others. Be specific about your safety duties."
- Examples link: Opens modal with sample descriptions

**Describe Overall Job Duties:**
- Multi-line text area (required) - from WORK_EXPERIENCE.overall_job_duties
- Character limit: 1000 characters
- Character counter display
- Help text: "Indicate duties performed in this position, focusing on safety-related activities"

**Action Buttons for Each Employer Entry:**
- Remove Employer: Button to delete this entry (only if more than one employer exists)
- Expand/Collapse: Toggle to show/hide full details

**Add Another Employer Button:**
- Prominent button at bottom of section
- "Add Another Employer" - adds new blank employer entry form

#### Available Actions and Buttons
- **Previous:** Returns to Step 2 (Personal Information), saves current data
- **Next:** Proceeds to Step 4 (Education & Certifications)
  - Validates all required fields for all employers
  - Validates date ranges are logical
  - Warns if total experience appears insufficient but allows proceeding
  - Saves data and advances wizard
- **Save as Draft:** Saves current data without validation
- **Cancel:** Returns to dashboard with confirmation dialog

#### Validation Rules
- At least one employer entry is required
- All fields within each employer entry are required
- Start date must be before end date (or "Present")
- Contact phone must match format (###) ###-####
- Contact email must be valid email format
- Percentage safety-related must be 0-100
- Hours per week safety cannot exceed total hours per week
- Total hours per week must be reasonable (1-168)
- Safety responsibilities must be at least 50 characters
- Overall job duties must be at least 50 characters
- Employment dates cannot overlap for same employer
- No future employment end dates allowed
- Warning (not blocking) if percentage safety-related is below 35% per business rule

#### Navigation
- **How users access this screen:** Next from Step 2 (Personal Information); Direct navigation if returning to draft; Previous from Step 4
- **Where users can go from here:** Previous → Step 2; Next → Step 4 (Education & Certifications); Cancel → Applicant Landing Page; Save as Draft → Applicant Landing Page

#### Related Data and Relationships
- Creates/updates multiple WORK_EXPERIENCE entity records (one per employer)
- Related to APPLICATION entity via APPLICATION.application_id
- Experience data used by system to calculate total qualifying years for prerequisite verification

#### Requirements Source
"List work experience with most recent employer first" (Appendix D - Page 2, line 288)

"For the prior employer section, all fields are required" (Validations, line 65)

"For field 17, provide a field for 'hours per week devoted to safety-related tasks' and a field for 'hours worked per week'" (Validations, line 68)

"35% cutoff for percentage of time spent on safety related tasks when listing prior employment (field 17); this cutoff is not transparent to the users and should not be explicitly revealed" (Notable Business Rules, line 87)

"A maximum of 2 years of experience can be substituted" (Validations, line 67) [by degree or certification, handled in next step]

#### Priority
High - Critical data collection for prerequisite verification, complex repeating section

---

### Screen 5: Application Wizard - Education & Certifications

#### Screen Type
Wizard (Step 4 of 6)

#### Target Persona
Applicants

#### Purpose and Business Context
Fourth step of wizard collecting education and professional certification information that can substitute for up to 2 years of required work experience. Corresponds to Section 40a and 40b of the official OSHA form (page 5). Only displayed if applicant wants to claim educational or certification substitution.

#### Interface Parameters
- **applicationId** (integer) - ID of application being created/edited
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Progress Indicator:**
- Step 4 of 6: Education & Certifications (current)
- Visual progress bar showing 67% complete
- Breadcrumb: Course Selection > Personal Information > Work Experience > Education & Certifications

**Instructions Section:**
- Header: "Education & Professional Certifications"
- Instruction Text: "A bachelor or higher degree in occupational safety and health or industrial hygiene from an accredited college/university, OR professional certification (CSP, CIH, CMC) may substitute for TWO (2) years of work experience."
- Note: "Complete this section only if you wish to substitute education or certification for work experience. You must provide proof of degree or certification."

**Substitution Option Selection:**
- Radio button options:
  - "I do not wish to substitute education or certification for experience" (skip to next step)
  - "I have a qualifying college degree" (shows College Degree section)
  - "I have a professional certification" (shows Professional Certification section)
  - "I have both a degree and certification" (shows both sections)

**College Degree Section:**
(Only visible if selected)

- Have Qualifying Degree: Checkbox "I have a degree in occupational safety and health or industrial hygiene from an accredited college or university" - from EDUCATION.has_degree
- College/University Name: Text input (required if section shown) - from EDUCATION.institution_name
- Academic Major: Dropdown (required) - from EDUCATION.degree_major
  - Options: "Occupational Safety and Health", "Industrial Hygiene", "Other (explain)"
  - If "Other": Text field appears for explanation
- Degree Level: Dropdown (required) - from EDUCATION.degree_level
  - Options: "Bachelor's", "Master's", "Doctorate"
- Date of Graduation: Date picker (mm/dd/yyyy) (required) - from EDUCATION.graduation_date
- Official Transcripts: File upload component (required) - creates SUPPORTING_DOCUMENT record
  - Accepted formats: PDF only
  - Help text: "You must attach official transcripts showing degree awarded with seal and signature"
  - File size limit: 10MB

**Professional Certification Section:**
(Only visible if selected)

- Certification Type: Checkbox group - from CERTIFICATION.certification_type
  - "Certified Safety Professional (CSP)"
  - "Certified Industrial Hygienist (CIH)"
  - "Certified Marine Chemist (CMC)" (only available if maritime course selected in Step 1)
  - "Certified Hazardous Materials Manager (CHMM)"
  - Multiple selections allowed

For each selected certification:
- Certifying Organization Name: Text input (required) - from CERTIFICATION.certifying_organization
- Certifying Organization Address: Text area (required) - from CERTIFICATION.organization_address
- Certification Number: Text input (required) - from CERTIFICATION.certification_number
- Issue Date: Date picker (required) - from CERTIFICATION.issue_date
- Expiration Date: Date picker (required) - from CERTIFICATION.expiration_date
  - Validation: Must not expire before course start date
  - Warning displayed if expiring within 30 days of course
- Certification Document: File upload (required) - creates SUPPORTING_DOCUMENT record
  - Accepted formats: PDF, JPG, PNG
  - Help text: "Upload copy of current professional certification card or certificate"
  - File size limit: 10MB per file

**Experience Substitution Calculator:**
- Years of Work Experience Submitted (from Step 3): X.XX years
- Years Substituted by Education/Certification: 2.00 years (if applicable)
- Total Qualifying Years: X.XX years
- Requirement for Selected Courses: Y years
- Status Indicator: Green checkmark if sufficient, yellow warning if borderline, red X if insufficient

#### Available Actions and Buttons
- **Previous:** Returns to Step 3 (Work Experience), saves current data
- **Next:** Proceeds to Step 5 (Prerequisites & Documents)
  - If substitution claimed, validates all required fields
  - Validates certification expiration dates
  - Validates documents are uploaded
  - Saves data and advances wizard
- **Save as Draft:** Saves current data without complete validation
- **Cancel:** Returns to dashboard with confirmation dialog
- **Skip This Step:** If "I do not wish to substitute" is selected, allows proceeding to next step immediately

#### Validation Rules
- If College Degree section shown:
  - All degree fields are required
  - Official transcript document must be uploaded (PDF only)
  - Graduation date must be in the past
  - Academic major must be qualifying field
- If Professional Certification section shown:
  - At least one certification type must be selected
  - All fields for each selected certification are required
  - Certification document must be uploaded for each certification
  - Expiration date must be after course start date(s) from Step 1
  - Certification number format validated based on certification type
- File uploads:
  - Maximum 10MB per file
  - Accepted formats: PDF for transcripts, PDF/JPG/PNG for certifications
  - File name cannot contain special characters

#### Navigation
- **How users access this screen:** Next from Step 3 (Work Experience); Direct navigation if returning to draft; Previous from Step 5
- **Where users can go from here:** Previous → Step 3; Next → Step 5 (Prerequisites & Documents); Skip → Step 5 (if no substitution); Cancel → Applicant Landing Page; Save as Draft → Applicant Landing Page

#### Related Data and Relationships
- Creates/updates EDUCATION entity record if degree claimed
- Creates multiple CERTIFICATION entity records if certifications claimed
- Creates SUPPORTING_DOCUMENT entity records for uploaded transcripts and certification documents
- Related to APPLICATION entity via APPLICATION.application_id
- Certification expiration dates validated against COURSE.start_date from selected courses

#### Requirements Source
"Complete this Section to Substitute Education or Professional Certification for Two (2) Years Work Experience" (Appendix D - Page 5, line 357)

"An approved degree (one of two specific majors) can substitute for 2 years of required experience" (Notable Business Rules, line 90)

"Credentials must not expire before course start date" (Notable Business Rules, line 91)

"COLLEGE DEGREE - PROOF REQUIRED... Attach required copy of official transcripts" (Appendix D, lines 358-364)

"PROFESSIONAL CERTIFICATION - PROOF REQUIRED... Attach required copy of current professional certification as a CSP, CIH, CMC" (Appendix D, lines 365-370)

"Additional Certification Verifications: The list of professional certifications to verify should include CHMM, CSP, CIH, and CMC" (Integrations section, line 131)

#### Priority
High - Critical for applicants using education/certification substitution pathway

---

### Screen 6: Application Wizard - Prerequisites & Documents

#### Screen Type
Wizard (Step 5 of 6)

#### Target Persona
Applicants

#### Purpose and Business Context
Fifth step of wizard where applicants document completion of prerequisite OSHA courses and upload supporting documentation. Dynamically displays required prerequisites based on courses selected in Step 1. Corresponds to Section 9 of the official OSHA form. Critical for ensuring only qualified applicants proceed.

#### Interface Parameters
- **applicationId** (integer) - ID of application being created/edited
- **language** (text) - "en" or "es" for English or Spanish display
- **selectedCourses** (array) - Courses selected in Step 1, determines which prerequisites to show

#### Data and Fields Displayed

**Progress Indicator:**
- Step 5 of 6: Prerequisites & Documents (current)
- Visual progress bar showing 83% complete
- Breadcrumb: Course Selection > Personal Information > Work Experience > Education & Certifications > Prerequisites & Documents

**Instructions Section:**
- Header: "Prerequisite Courses & Supporting Documentation"
- Instruction Text: "Based on the course(s) you selected, you must provide evidence of completing the prerequisite courses listed below. Upload completion certificates or cards for each prerequisite course. Courses must have been completed within the last 7 years."
- Important Note: "For update courses (#502, #503, #5402, #5602): You may provide either your current OSHA Outreach Training Program trainer card OR an official transcript from an OTI Education Center"

**Dynamic Prerequisites Display:**
Based on courses selected in Step 1, system displays required prerequisites:

**For OSHA #500 (if selected):**
- Prerequisite: "OSHA #510 - Occupational Safety and Health Standards for the Construction Industry"
- Required checkbox: "I have completed OSHA #510" - from PREREQUISITE_COURSE.is_completed
- Course Completion Date: Date picker (required) - from PREREQUISITE_COURSE.completion_date
  - Validation: Must be within last 7 years from today
  - Warning if close to 7-year expiration
- Issuing OTI Education Center: Dropdown or text input (required) - from PREREQUISITE_COURSE.issuing_center
- Certificate/Card Number: Text input (optional) - from PREREQUISITE_COURSE.certificate_number
- Upload Certificate: File upload (required) - creates SUPPORTING_DOCUMENT
  - Accepted formats: PDF, JPG, PNG
  - Help text: "Upload copy of OSHA #510 completion certificate or card"
  - Visual indicator of upload status

**For OSHA #501 (if selected):**
- Prerequisite: "OSHA #511 - Occupational Safety and Health Standards for General Industry"
- [Same fields as above for #510]

**For OSHA #5400 (if selected):**
- Prerequisite: "OSHA #5410 - Occupational Safety and Health Standards for the Maritime Industry"
- [Same fields as above]

**For OSHA #5600 (if selected):**
- Multiple prerequisites required:
  - Prerequisite 1: "Current OSHA authorization as Construction, Maritime, or General Industry Outreach trainer"
    - Upload Current Trainer Card (required)
    - Expiration date must be validated
  - Prerequisite 2: "Three years of safety training experience" (already captured in Step 3)
  - Prerequisite 3: Choose one:
    - Radio option 1: "40-hour HAZWOPER course completion"
      - Upload HAZWOPER certificate (required if selected)
      - Completion date (required)
    - Radio option 2: "Journey-level credentials in building trade union"
      - Upload credentials document (required if selected)
      - Union name (required)
      - Credential type (required)

**For Update Courses (#502, #503, #5402, #5602) (if selected):**
- Special Option Display: "You may provide ONE of the following:"
  - Radio option 1: "Current OSHA Outreach Training Program trainer card"
    - Upload trainer card (required if selected)
    - Expiration date (required, must be current within 10 years)
    - Card number (optional)
  - Radio option 2: "Official transcript from OTI Education Center for prerequisite course"
    - Upload official transcript (required if selected)
    - OTI Education Center name (required)
    - Course number completed (required - #500, #501, or #5400)
    - Completion date (required)

**Previously Authorized Trainers Section:**
(Only shown if applicant indicates this applies)
- Checkbox: "I am a previously authorized Outreach Training Program trainer"
- If checked:
  - Upload Outreach trainer card (required)
  - Expiration date (required, must be no more than 10 years old)
  - OR: Upload official transcript from OTI Education Center
  - Help text: "If your trainer card is not available, you must provide an official transcript"

**Document Upload Summary:**
- List of all uploaded documents with status:
  - Document name
  - File type
  - Upload date/time
  - Status: Uploaded (green checkmark)
  - Remove button for each document
  - Preview button to view document

**Additional Supporting Documentation Section:**
- Optional uploads for any additional supporting documents
- Multi-file upload component
- Help text: "You may upload any additional documents that support your prerequisites (optional)"

#### Available Actions and Buttons
- **Previous:** Returns to Step 4 (Education & Certifications), saves current data
- **Next:** Proceeds to Step 6 (Review & Sign)
  - Validates all required prerequisites are documented
  - Validates completion dates are within 7-year window
  - Validates all required documents are uploaded
  - Saves data and advances wizard
- **Save as Draft:** Saves current data without complete validation
- **Cancel:** Returns to dashboard with confirmation dialog
- **Preview Document:** Opens modal with document preview for uploaded files
- **Remove Document:** Deletes uploaded document with confirmation

#### Validation Rules
- All prerequisites determined by selected courses must be documented
- Each prerequisite requires:
  - Completion checkbox must be checked
  - Completion date must be provided and within valid date range
  - Certificate/card document must be uploaded
- Completion date validations:
  - OSHA #510, #511, #5410 must be completed within last 7 years (per business rule)
  - Trainer cards must not be expired more than 10 years
  - HAZWOPER certificates should be current (warning if older than 1 year)
- File upload validations:
  - Accepted formats: PDF, JPG, PNG
  - Maximum 10MB per file
  - Each required prerequisite must have at least one document
- For update courses: Must select one option (trainer card OR transcript) and provide required documentation
- System validates certificate/transcript documents have seal and signature (manual review note added if unclear)

#### Navigation
- **How users access this screen:** Next from Step 4 (Education & Certifications); Direct navigation if returning to draft; Previous from Step 6
- **Where users can go from here:** Previous → Step 4; Next → Step 6 (Review & Sign); Cancel → Applicant Landing Page; Save as Draft → Applicant Landing Page

#### Related Data and Relationships
- Creates multiple PREREQUISITE_COURSE entity records (one per prerequisite course)
- Creates multiple SUPPORTING_DOCUMENT entity records (one per uploaded certificate)
- Related to APPLICATION entity via APPLICATION.application_id
- Related to APPLICATION_COURSE entity (links prerequisites to specific courses applied for)
- Completion dates validated against COURSE.start_date (must be within 7 years)

#### Requirements Source
"I have completed the following prerequisite course(s). (Attach a copy of the course completion card or certificate for each applicable course)" (Appendix D - Page 1, line 261)

"Key prerequisite courses, such as the OSHA 510 and 511, are only valid for seven years. The system must validate that the completion date of these submitted certificates is within this seven-year window" (Notable Business Rules, line 92)

"For Previously Authorized Trainers – If the student is a previously authorized Outreach Training Program trainer, a copy of the Outreach trainer card with an expiration date no more than ten (10) years old may be accepted. If the Outreach Training Program trainer card is not available, an official transcript from an OTI Education Center must be provided" (Appendix B, line 221)

"OSHA #5600 Disaster Site Worker Trainer Course - Current OSHA authorization as a Construction, Maritime, or General Industry Outreach trainer, three years of safety training experience, and either completion of the 40-hour HAZWOPER course or possession of journey-level credentials in a building trade union" (Appendix B, line 220)

#### Priority
High - Critical validation step, ensures prerequisite requirements are met before submission

---

### Screen 7: Application Wizard - Review & Sign

#### Screen Type
Wizard (Step 6 of 6)

#### Target Persona
Applicants

#### Purpose and Business Context
Final step of wizard where applicants review all entered information, see preview of generated OSHA Prerequisite Verification PDF(s), and digitally sign their application(s) using DocuSign. This step ensures accuracy and creates the official signed documents required for audit purposes. Corresponds to Statement of Certification section (page 5) of official form.

#### Interface Parameters
- **applicationId** (integer) - ID of application being completed
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Progress Indicator:**
- Step 6 of 6: Review & Sign (current)
- Visual progress bar showing 100% complete
- Breadcrumb: Course Selection > Personal Information > Work Experience > Education & Certifications > Prerequisites & Documents > Review & Sign

**Instructions Section:**
- Header: "Review & Sign Your Application"
- Instruction Text: "Please carefully review all information below. This information will appear on your official OSHA Prerequisite Verification Form(s). You will digitally sign your application using DocuSign. Once submitted, your application will be sent to USF OSHA Training Institute Education Center for review."
- Important Note: "You are creating [X] separate application(s), one for each course selected. Each application will have its own signed PDF."

**Application Summary - Collapsible Sections:**

**Section 1: Selected Courses**
- Display list of all selected courses from Step 1:
  - Course Number
  - Course Name
  - Start Date
  - End Date
  - Location
  - Contract Course indicator (if applicable)
- Edit link → returns to Step 1

**Section 2: Personal Information**
- Display all fields from Step 2:
  - Legal Name
  - Job Title
  - Company
  - Email
  - Phone
  - Fax
  - Mailing Address (full)
  - Previously Subject to Revocation: Yes/No
- Edit link → returns to Step 2

**Section 3: Work Experience**
- Display summary for each employer from Step 3:
  - Employer name, job title, dates
  - Years/months in position
  - Percentage safety-related
  - Brief preview of safety responsibilities (first 100 characters)
  - Expand to see full details
- Total Years of Experience: X.XX years
- Edit link → returns to Step 3

**Section 4: Education & Certifications**
- If applicable, display from Step 4:
  - Degree information (institution, major, level, graduation date)
  - Certification information (type, organization, expiration date)
  - Years substituted: 2.00 years
- If not applicable: "No education or certification substitution claimed"
- Edit link → returns to Step 4

**Section 5: Prerequisites & Supporting Documents**
- Display list of all documented prerequisites from Step 5:
  - Prerequisite course name
  - Completion date
  - Issuing center
  - Document uploaded: Filename
- Total documents uploaded: X
- Edit link → returns to Step 5

**Total Qualification Summary:**
- Years of Work Experience: X.XX
- Years Substituted (if applicable): 2.00
- Total Qualifying Years: X.XX
- Required Years for Selected Courses: Y
- Status: ✓ Qualified / ⚠ May Need Additional Review / ✗ Insufficient (warning, does not block submission)

**Generated PDF Previews:**
For each course selected, display:
- Preview thumbnail of generated OSHA Prerequisite Verification PDF
- Course: [Course number and name]
- PDF Name: "OSHA_Prerequisite_Verification_[LastName]_[CourseNumber].pdf"
- View Full PDF button → opens PDF in modal or new tab
- Status: "Ready for signature"

**Statement of Certification:**
- Display full certification text:
  "I certify that the information I have included herein and submitted to the OTI Education Center is true and accurate. I understand that I will be subject to immediate dismissal from the OSHA Outreach Training Program if information provided herein is not true and correct. I further understand that providing false information herein may subject me to civil and criminal penalties under Federal law, including 18 U.S.C. 1001 and section 17(g) of the Occupational Safety and Health Act, 29 U.S.C. 666 (g), which provides criminal penalties for making false statements or representations in any document filed pursuant to that Act."
- Checkbox: "I have read and agree to the Statement of Certification" (required to enable Submit button)
- Digital Signature Note: "You will sign electronically using DocuSign when you click Submit & Sign"

#### Available Actions and Buttons
- **Previous:** Returns to Step 5 (Prerequisites & Documents), saves any changes
- **Edit [Section]:** Multiple edit links throughout the review, each returns to the corresponding wizard step
- **View Full PDF:** Opens modal displaying complete generated PDF for review
- **Submit & Sign:** Primary action button
  - Disabled until "I have read and agree" checkbox is checked
  - Initiates DocuSign e-signature process
  - Creates DocuSign envelope with all generated PDFs
  - Sends to applicant's email for signature
  - Opens DocuSign signing interface in modal or redirects to DocuSign
  - On signature completion:
    - Updates APPLICATION.status to "Submitted"
    - Stores signed PDFs in Appian
    - Sends confirmation email to applicant
    - Sends application to Approver queue
    - Redirects to Application Status View with success message
- **Save as Draft:** Saves current state, allows returning later (application not submitted, PDFs not signed)
- **Cancel:** Returns to dashboard with confirmation dialog

#### Validation Rules
- All previous wizard steps must be complete with valid data
- "I have read and agree to Statement of Certification" must be checked
- All generated PDFs must be successfully created (system validation)
- Applicant email must be valid for DocuSign delivery
- System validates 2-week deadline before submission (course start date must be at least 14 days away)

#### Navigation
- **How users access this screen:** Next from Step 5 (Prerequisites & Documents); Direct navigation if returning to draft at this step
- **Where users can go from here:**
  - Previous → Step 5
  - Edit links → Steps 1-5 (returns to specific step)
  - After successful submission → Application Status View
  - Cancel → Applicant Landing Page

#### Related Data and Relationships
- Reads from all previously created/updated entities to display summary
- Updates APPLICATION entity:
  - APPLICATION.status changes to "Submitted"
  - APPLICATION.submitted_date timestamp
  - APPLICATION.docusign_envelope_id stored
- Creates signed PDF documents stored as SUPPORTING_DOCUMENT entities
- Triggers notification to APPROVER role
- Creates audit log entry for submission

#### Requirements Source
"When the applicant is ready to submit, we generate a pdf for each course they are applying for, and have them digitally sign the application" (Application Submission, line 72)

"Statement of Certification: This statement must be signed by the applicant to certify that the information provided on the Prerequisite Verification Form is true and correct. Neglecting to sign the Statement of Certification will result in the application being declined" (Appendix D, line 496-497)

"The system will also provide automated email confirmations upon successful submission" (Description of To-Be, line 48)

"There must be individual, signed PDFs for each course an applicant applied for" (Auditability, line 184)

#### Priority
High - Final critical step in application workflow, creates official signed documents

---


### Screen 8: Application Status View

#### Screen Type
Record Instance

#### Target Persona
Applicants

#### Purpose and Business Context
Provides applicants with detailed view of a specific application's current status, review history, and any comments from reviewers. Reduces applicant anxiety by providing transparency into the review process. Allows applicants to respond to requests for additional information and track all communications related to their application.

#### Interface Parameters
- **applicationId** (integer) - ID of the application to display
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Application Header:**
- Application Number: Text, unique identifier from APPLICATION.application_number
- Submission Date: Date/time from APPLICATION.submitted_date
- Current Status Badge: Color-coded badge from APPLICATION.status
  - Submitted = Blue
  - Under Review = Yellow
  - Returned for Information = Orange
  - Approved = Green
  - Denied = Red

**Course Information Section:**
- List of courses applied for:
  - Course Number (e.g., OSHA #500)
  - Course Name
  - Start Date
  - End Date
  - Location
  - Individual Course Status (if different from overall application status)

**Application Timeline:**
Visual timeline display showing:
- Application Started: Date/time
- Application Submitted: Date/time
- Under Review: Date/time (if applicable)
- Returned for Information: Date/time and count (if applicable)
- Re-submitted: Date/time (if applicable)
- Final Decision: Date/time with Approved/Denied status

**Current Status Details:**
Dynamic section based on APPLICATION.status:

**If Status = "Submitted":**
- Message: "Your application has been received and is awaiting review"
- Expected Review Time: "Applications are typically reviewed within 5-7 business days"
- Next Steps: "You will receive an email notification when your application review begins"

**If Status = "Under Review":**
- Message: "Your application is currently being reviewed by [Reviewer Name]"
- Review Started: Date/time
- Next Steps: "The reviewer will approve, request additional information, or make a decision"

**If Status = "Returned for Information":**
- Message: "The reviewer has requested additional information"
- Reviewer Comments: Display full text from APPLICATION_COMMENT.comment_text
- Date Returned: Date/time
- Action Required Badge: "Your Response Needed"
- Respond Button: Opens response form (inline or modal)

**If Status = "Approved":**
- Message: "Congratulations! Your application has been approved"
- Approval Date: Date/time from APPLICATION.approval_date
- Approved By: Reviewer name
- Enrollment Instructions: Display enrollment link and registration code
- Enrole Registration Link: Clickable URL
- Registration Code: Display code, copy-to-clipboard button
- Next Steps: Instructions for enrolling in course

**If Status = "Denied":**
- Message: "Your application has been denied"
- Denial Date: Date/time
- Reason for Denial: Text from APPLICATION.denial_reason
- Appeal Information: Link to contact information or appeal process
- What You Can Do: Guidance on next steps

**Reviewer Comments History:**
Chronological list of all comments:
- Comment Date: Date/time stamp
- Comment Type: Badge (Information Request / Clarification / Decision Note)
- Reviewer: Name of reviewer who made comment
- Comment Text: Full comment content
- Applicant Response: If applicant has responded, show response
- Attachments: Any documents attached to comment

**Your Response Section:**
(Only visible if status = "Returned for Information")
- Response Text Area: Rich text editor for applicant response
- Character limit: 2000 characters
- Attach Documents: File upload component
  - Accepted formats: PDF, JPG, PNG, DOC, DOCX
  - Max 10MB per file, max 5 files
  - Help text: "Upload any additional documents requested by the reviewer"
- Preview Response: Shows how response will appear to reviewer

**Application Documents:**
List of all documents associated with application:
- Document Name/Type
- Upload Date
- File Size
- Download Link
- Preview Button

**Signed PDFs:**
- OSHA Prerequisite Verification Form(s): One per course
  - Download PDF button
  - View PDF button (opens in modal)

#### Available Actions and Buttons
- **Download Application PDF:** Downloads signed OSHA Prerequisite Verification Form
- **Download All Documents:** Downloads ZIP file of all application documents
- **Submit Response:** (Only visible if status = "Returned for Information")
  - Validates response text is provided
  - Uploads any attached documents
  - Changes status to "Re-submitted"
  - Sends notification to reviewer
  - Updates timeline
- **Edit Application:** (Only visible if status = "Returned for Information")
  - Re-opens application wizard at appropriate step
  - Allows editing previously submitted information
  - Requires re-signature if substantial changes made
- **Withdraw Application:** Button to cancel/withdraw application
  - Confirmation dialog required
  - Changes status to "Withdrawn"
  - Sends notification to reviewer if under review
- **Print Application Summary:** Generates printable version of status page
- **Contact Support:** Opens email to support with application number pre-filled

#### Validation Rules
- Response text must be at least 20 characters when submitting response
- Uploaded documents must meet file type and size requirements
- Cannot submit response if no text or documents provided
- Cannot edit or withdraw application once approved or denied

#### Navigation
- **How users access this screen:**
  - Click application number from Applicant Landing Page
  - Click application from My Applications List
  - Direct link from email notifications
- **Where users can go from here:**
  - Back to My Applications List
  - Back to Applicant Landing Page
  - Edit Application (re-opens wizard)
  - Download documents

#### Related Data and Relationships
- Displays APPLICATION entity record
- Related to APPLICATION_COURSE entities (shows all courses on application)
- Related to COURSE entities (displays course details)
- Related to APPLICATION_COMMENT entities (displays all comments and responses)
- Related to SUPPORTING_DOCUMENT entities (displays all uploaded documents)
- Related to APPLICANT entity (displays applicant information)
- Related to USER entity (displays reviewer information)

#### Requirements Source
"Applicants will have access to a self-service status page to check on the progress of their application, reducing the need for follow-up emails" (Description of To-Be, line 48)

"Applications that were sent back for review are not closed automatically. Resubmissions from applicants shall be tracked on the same application case" (Watch List Management section, line 85)

"Applicant Anxiety and Follow-Up: Applicants frequently email to check the status of their application because they don't receive an automatic confirmation and are unsure if their submission was successful" (Description of As-Is Pain Points, line 41)

#### Priority
High - Critical for applicant self-service and reducing support burden

---

### Screen 9: My Applications List

#### Screen Type
Record List

#### Target Persona
Applicants

#### Purpose and Business Context
Displays all applications submitted by the logged-in applicant in a sortable, filterable grid. Allows applicants to track multiple applications across different courses and time periods. Provides quick overview of status and actions needed.

#### Interface Parameters
- **language** (text) - "en" or "es" for English or Spanish display

#### Data and Fields Displayed

**Page Header:**
- Title: "My Applications"
- Total Applications Count: Display count
- Actions Needed Count: Highlighted count of applications requiring response

**Filter and Search Controls:**
- Search Box: Search by course number, course name, or application number
- Status Filter: Multi-select dropdown
  - All Statuses
  - Draft
  - Submitted
  - Under Review
  - Returned for Information
  - Approved
  - Denied
  - Withdrawn
- Date Range Filter: Date pickers for submission date range
- Course Filter: Dropdown of all courses applicant has applied for
- Clear All Filters: Button to reset filters

**Applications Grid:**
Columns:
- Application Number: Linked to Application Status View, from APPLICATION.application_number
- Submission Date: Date from APPLICATION.submitted_date, sortable
- Course(s): Text list of course numbers, from related APPLICATION_COURSE records
- Status: Badge with color coding from APPLICATION.status, filterable
- Last Updated: Date/time from APPLICATION.last_modified_date, sortable
- Action Required: Icon/badge if status = "Returned for Information"
- Actions: Row action menu
  - View Details
  - Download PDF
  - Continue (if Draft)
  - Respond (if Returned for Information)
  - Withdraw (if applicable)

**Grid Features:**
- Sortable columns (click column header to sort)
- Pagination: 20 applications per page
- Row selection: Checkbox for bulk actions
- Empty state: "You have not submitted any applications yet. Start your first application!"

**Bulk Actions:**
(Visible when rows selected)
- Download Selected PDFs
- Export Selected to CSV

**Quick Stats Panel:**
(Optional sidebar or top section)
- Applications by Status: Pie chart or count badges
- Recent Activity: Last 3 status changes across all applications
- Upcoming Course Dates: For applications with status = Approved

#### Available Actions and Buttons
- **View Details:** Opens Application Status View for selected application
- **Download PDF:** Downloads signed OSHA Prerequisite Verification Form
- **Continue Draft:** Opens application wizard at last saved step (only for Draft status)
- **Respond to Request:** Opens Application Status View with response section expanded (only for Returned for Information status)
- **Withdraw Application:** Cancels application with confirmation dialog
- **New Application:** Primary button to start new application wizard
- **Export to CSV:** Exports filtered list to CSV file
- **Print List:** Generates printable version of application list

#### Validation Rules
N/A (read-only list)

#### Navigation
- **How users access this screen:**
  - "View All My Applications" link from Applicant Landing Page
  - Navigation menu item "My Applications"
  - Back button from Application Status View
- **Where users can go from here:**
  - Application Status View (click application number or View Details)
  - Application Wizard (Continue Draft or New Application)
  - Applicant Landing Page (navigation or back)

#### Related Data and Relationships
- Displays APPLICATION entity records filtered by current APPLICANT
- Related to APPLICATION_COURSE entities (shows courses per application)
- Related to COURSE entities (displays course information)
- Sorted by APPLICATION.submitted_date or APPLICATION.last_modified_date

#### Requirements Source
"Applicants will have access to a self-service status page to check on the progress of their application" (Description of To-Be, line 48)

"For applicants applying to multiple courses, the interface will allow for a seamless, single-session application process" (Description of To-Be, line 48)

#### Priority
Medium - Important for multi-application management, especially for repeat applicants

---

### Screen 10: Approver Dashboard

#### Screen Type
Landing Page

#### Target Persona
Approvers

#### Purpose and Business Context
Main entry point for approvers after login. Provides high-level overview of application review workload, prioritizes applications requiring attention, displays key metrics, and offers quick access to review queues and tools. Helps approver manage review workload efficiently.

#### Interface Parameters
N/A (landing page for Approver persona)

#### Data and Fields Displayed

**Welcome Section:**
- Approver Name: Greeting with user's name
- Last Login: Date/time of previous login
- Current Date/Time: Display for record-keeping

**Workload Summary Cards:**
- Applications Awaiting Review: Count of applications with status = "Submitted"
  - Link to Applications Queue filtered by "Submitted"
  - Priority indicator if any applications approaching 2-week deadline
- Applications Currently Under Your Review: Count of applications with status = "Under Review" assigned to current approver
  - Link to Applications Queue filtered by current user
- Applications Returned to Applicants: Count of applications with status = "Returned for Information"
  - Tracking for follow-up
- Applications Needing Re-Review: Count of applications with status = "Re-submitted"
  - High priority indicator
  - Link to Applications Queue filtered by "Re-submitted"

**Priority Alerts:**
- Urgent Reviews: List of applications with course start dates within 3 weeks
  - Application Number
  - Applicant Name
  - Course(s)
  - Days Until Course Start: Countdown
  - Quick Action: "Review Now" button
- Incomplete Reviews: Applications you started reviewing more than 3 days ago
  - Application Number
  - Days Since Review Started
  - Quick Action: "Continue Review" button

**Review Metrics (Current Month):**
- Applications Reviewed: Count
- Average Review Time: Hours/days
- Approval Rate: Percentage
- Applications Returned for Information: Count and percentage
- Denials: Count and percentage
- Historical comparison: Up/down arrow vs. previous month

**Recent Activity Feed:**
- Last 10 actions across all applications:
  - Timestamp
  - Application Number: Linked
  - Applicant Name
  - Action: (Submitted, Approved, Denied, Returned, Re-submitted)
  - Course(s)

**Quick Actions Section:**
- Review Next Application: Button to open oldest unreviewed application
- View Full Applications Queue: Link to Applications Queue screen
- Check Watch Lists: Link to Watch List Verification screen
- View Reports: Link to reporting screens
- Update Watch List: Link to Watch List Management (admin function, if approver has admin rights)

**Helpful Resources:**
- Prerequisite Requirements Guide: Link to reference document
- External Verification Links: Quick links to external watchlists and certification verification sites
  - OSHA Outreach Trainer Watch List
  - Board of Certified Safety Professionals
  - Marine Chemist Association
  - ABIH Certification Search
- Application Review Checklist: Link to internal checklist/guidance
- Contact Applicant Email Templates: Pre-formatted email templates

#### Available Actions and Buttons
- **Review Next Application:** Opens Application Review Interface for next pending application
- **View Applications Queue:** Navigates to Applications Queue screen
- **Quick Review:** Hover action on priority alerts to open application in modal
- **Export Metrics:** Downloads current month metrics to CSV
- **View All Activity:** Expands recent activity feed to show more items

#### Validation Rules
N/A (informational dashboard)

#### Navigation
- **How users access this screen:**
  - Default landing page after login for Approver persona
  - Accessible via site navigation "Home" or "Dashboard" link
- **Where users can go from here:**
  - Applications Queue (Record List)
  - Application Review Interface (via Review Now or Review Next buttons)
  - Watch List Verification Screen
  - Reports and dashboards

#### Related Data and Relationships
- Aggregates data from APPLICATION entity
- Filters by APPLICATION.status for different queues
- Related to USER entity (tracks current approver's assignments)
- Calculates metrics from APPLICATION entity timestamps and status changes
- Links to COURSE entity for course start date urgency calculations

#### Requirements Source
"Approvers: Trained USF employees who review applications and determine whether the Applicant is qualified to take the course" (Key Actors, line 51)

"Application Review: Once submitted, the application will be routed to the approver for review. The approver will be able to see all the information in one place and make a decision" (Application Review section, line 73)

"2 week cutoff for applications prior to course start date" (Notable Business Rules, line 88) - implies need to prioritize applications approaching deadline

#### Priority
High - Primary entry point for approver persona, critical for workload management

---


### Screen 11: Applications Queue (Record List)

#### Screen Type
Record List

#### Target Persona
Approvers

#### Purpose and Business Context
Comprehensive list view of all applications across all statuses with robust filtering, sorting, and search capabilities. Allows approvers to find specific applications, prioritize reviews based on various criteria, and manage review workload efficiently. Serves as central hub for application review workflow.

#### Interface Parameters
N/A (record list for Approver persona)

#### Data and Fields Displayed

**Page Header:**
- Title: "Applications Queue"
- Total Applications Count: Display total
- Filtered Count: Shows count after filters applied

**Filter and Search Controls:**
- Global Search: Search across application number, applicant name, email, company
- Status Filter: Multi-select dropdown
  - Submitted (default selected)
  - Under Review
  - Returned for Information
  - Re-submitted
  - Approved
  - Denied
  - Withdrawn
  - Show All
- Course Filter: Multi-select dropdown of all courses
- Submission Date Range: Date pickers (from/to)
- Course Start Date Range: Date pickers to find applications for specific course dates
- Assigned Reviewer: Dropdown filter (All Reviewers / Me / Specific Reviewer / Unassigned)
- Priority Filter: Dropdown
  - All
  - Urgent (course starts within 3 weeks)
  - Normal
- Experience Issues: Checkbox "Show only applications with insufficient experience warnings"
- Watch List Flags: Checkbox "Show only applications with watch list flags"
- Save Filter Set: Save current filters as named preset
- Clear All Filters: Reset to default view

**Applications Grid:**
Columns:
- Priority Icon: Red flag for urgent, yellow for approaching deadline
- Application Number: Linked to Application Review Interface, from APPLICATION.application_number
- Applicant Name: Text from APPLICANT.legal_name, linked to application
- Company: Text from APPLICATION.company_name
- Course(s): Badge list of course numbers from APPLICATION_COURSE
- Submission Date: Date from APPLICATION.submitted_date, sortable
- Course Start Date: Earliest start date from selected courses, sortable
- Days to Start: Calculated countdown, sortable
- Status: Badge with color coding from APPLICATION.status
- Assigned Reviewer: User name or "Unassigned"
- Flags: Icon indicators
  - Watch list flag (red exclamation if on watch list)
  - Experience warning (yellow triangle if questionable experience)
  - Missing documents (document icon if incomplete)
- Last Activity: Date/time of most recent action
- Actions: Row action menu
  - Review/Continue Review
  - Assign to Me
  - Assign to Another Reviewer
  - View Application
  - Download PDF
  - Add Comment

**Grid Features:**
- Sortable columns (click header to sort ascending/descending)
- Multi-select rows with checkboxes
- Configurable columns (show/hide columns preference)
- Pagination: 50 applications per page, configurable
- Export to CSV/Excel
- Row highlight: Color-coded by priority
  - Red background: Urgent (< 2 weeks to course start)
  - Yellow background: Approaching deadline (2-3 weeks)
  - White background: Normal

**Bulk Actions Panel:**
(Appears when rows selected)
- Assign Selected to Me
- Assign Selected to: Dropdown of reviewers
- Download Selected PDFs
- Export Selected to CSV
- Mark as Priority

**Quick View Panel:**
(Optional side panel or modal)
- Click on row shows quick preview without full navigation:
  - Applicant summary
  - Courses applied for
  - Current status and timeline
  - Quick actions: Approve, Return, Deny
  - Full Review button

#### Available Actions and Buttons
- **Review Application:** Opens Application Review Interface for selected application
- **Assign to Me:** Claims application for review (sets assigned_reviewer to current user)
- **Assign to Another Reviewer:** Opens dialog to select reviewer from dropdown
- **View Application:** Opens Application Review Interface in read-only mode
- **Download PDF:** Downloads signed OSHA Prerequisite Verification Form
- **Add Comment:** Opens inline comment field to add note (visible to other reviewers, not applicant)
- **Export to CSV/Excel:** Exports filtered grid data
- **Refresh List:** Reloads data to show latest updates
- **Configure Columns:** Opens dialog to show/hide columns
- **Save View:** Saves current filters and column configuration as named view

#### Validation Rules
N/A (read-only list with assignment actions)

#### Navigation
- **How users access this screen:**
  - "View Applications Queue" from Approver Dashboard
  - Navigation menu item "Applications Queue"
  - Direct links from dashboard filtered by status
- **Where users can go from here:**
  - Application Review Interface (click application number or Review action)
  - Approver Dashboard (navigation or back)
  - Watch List Verification Screen (if reviewing flagged application)

#### Related Data and Relationships
- Displays APPLICATION entity records with all statuses
- Joined with APPLICANT entity (displays applicant information)
- Joined with APPLICATION_COURSE entity (displays courses)
- Joined with COURSE entity (displays course dates for priority calculation)
- Related to USER entity (displays assigned reviewer)
- Related to WATCH_LIST entity (flags applications with watch list matches)

#### Requirements Source
"Application Review: Once submitted, the application will be routed to the approver for review. The approver will be able to see all the information in one place and make a decision" (Application Review section, line 73)

"There is not currently any reporting, but the customer is excited about the opportunity to get the kind of common reports you might expect... Task Report: Tracks in-progress applications" (Reports section, line 155)

#### Priority
High - Central workflow screen for approver persona, critical for review management

---

### Screen 12: Application Review Interface

#### Screen Type
Record Instance (with embedded form actions)

#### Target Persona
Approvers

#### Purpose and Business Context
Comprehensive application review screen where approvers see all application information, verify prerequisites, check external systems, and make approval decisions. Provides all necessary context in one place to make informed decisions. Supports the three-action model: Approve, Return for Information, or Deny.

#### Interface Parameters
- **applicationId** (integer) - ID of application being reviewed
- **viewMode** (text) - "review" for active review, "readonly" for viewing completed reviews

#### Data and Fields Displayed

**Application Header:**
- Application Number: Large, prominent display
- Status Badge: Current status
- Priority Indicator: Urgent flag if course starts soon
- Assigned Reviewer: Current reviewer name (or assign to me button if unassigned)
- Days Until Course Start: Countdown with color coding

**Quick Navigation Tabs:**
- Overview
- Work Experience Details
- Education & Certifications
- Prerequisites & Documents
- Verification & Checks
- History & Comments

**Tab 1: Overview**

**Applicant Information:**
- Legal Name
- Job Title
- Company
- Email (with mailto link)
- Phone (with click-to-call)
- Fax
- Mailing Address (full)

**Courses Applied For:**
Table showing:
- Course Number
- Course Name
- Start Date
- End Date
- Location
- Contract Course: Yes/No
- Registration Code: (if contract course, display and validate)

**Prerequisite Summary:**
For each course:
- Required Prerequisites: List
- Status: Complete/Incomplete indicator for each
- Years of Experience Required: X years
- Years of Experience Submitted: Y years
- Education/Certification Substitution: Yes/No, details if yes
- Qualification Status: ✓ Qualified / ⚠ Needs Review / ✗ Does Not Meet Requirements

**Previous Revocation/Suspension:**
- Has Previous Revocation: Yes/No
- If Yes: List of attached OSHA correspondence documents with download links
- Reviewer Note Field: Add internal notes about revocation review

**Application Timeline:**
- Submitted: Date/time
- Review Started: Date/time
- Returned for Information: Date/time and count (if applicable)
- Re-submitted: Date/time (if applicable)

**Tab 2: Work Experience Details**

For each employer submitted:

**Employer [X] of [Total]:**
- Employer Name and Job Title
- Employment Dates: Start to End (or Present)
- Duration: X years, Y months
- Contact Person Information:
  - Name
  - Phone (click-to-call)
  - Email (mailto link)
- Company Address (full)
- Percentage Safety-Related: X% (highlighted in red if < 35%)
- Hours per Week on Safety: X hours
- Total Hours per Week: Y hours
- Calculated Percentage: Z% (auto-calculated for verification)

**Safety Responsibilities:**
- Full text display of safety responsibilities description
- Character count: X characters
- Reviewer Assessment Checkboxes:
  - ☐ Responsibilities are clearly described
  - ☐ Responsibilities are relevant to course applied for
  - ☐ Responsibilities demonstrate required experience level

**Overall Job Duties:**
- Full text display of overall job duties
- Reviewer Assessment Checkboxes:
  - ☐ Duties align with safety responsibilities
  - ☐ Duties are appropriate for experience qualification

**Experience Calculation for this Employer:**
- Years in Position: X.XX years
- Percentage Safety-Related: Y%
- Qualifying Years from This Employer: Z.ZZ years
- Notes field for reviewer

**Total Experience Summary:**
(At bottom of tab)
- Total Years Submitted: X.XX years
- Total Qualifying Years (weighted by % safety): Y.YY years
- Required Years for Course(s): Z years
- Status: ✓ Sufficient / ⚠ Borderline / ✗ Insufficient

**Tab 3: Education & Certifications**

**Education Section:**
(If claimed)
- Has Qualifying Degree: Yes/No
- Institution Name
- Academic Major
- Degree Level
- Graduation Date
- Transcript Status: Uploaded/Not Uploaded
- Download Transcript: Link
- View Transcript: Button (opens in modal)

**Reviewer Verification Checklist - Education:**
- ☐ Transcript shows seal and signature
- ☐ Degree is in qualifying field (occupational safety/industrial hygiene)
- ☐ Institution is accredited
- ☐ Transcript is official (not unofficial copy)
- ☐ Approved for 2-year substitution
- Notes field

**Professional Certifications Section:**
(For each certification claimed)

**Certification: [Type]**
- Certification Type: CSP, CIH, CMC, or CHMM
- Certifying Organization
- Organization Address
- Certification Number
- Issue Date
- Expiration Date (highlighted in red if expired or expires before course start)
- Days Until Expiration: Countdown
- Certification Document: Download/View links

**External Verification Links:**
Based on certification type, display quick links:
- CSP: Link to Board of Certified Safety Professionals directory
- CIH/CHMM: Link to ABIH certification search
- CMC: Link to Marine Chemist Association directory
- Instructions: "Open link in new tab to verify certification"

**Reviewer Verification Checklist - Certification:**
- ☐ Certification number verified in external system
- ☐ Certification is current (not expired)
- ☐ Certification will not expire before course start date
- ☐ Certification document is clear and legible
- ☐ Approved for 2-year substitution
- Notes field
- Verification Date: Auto-filled when checklist completed

**Tab 4: Prerequisites & Documents**

For each prerequisite course required:

**Prerequisite Course: [Course Number and Name]**
- Required By: List of courses that require this prerequisite
- Completion Date: Date submitted
- Years Since Completion: Calculated
- Status: ✓ Within 7 years / ✗ Expired (if > 7 years)
- Issuing OTI Education Center
- Certificate Number (if provided)
- Certificate Document: Download/View links
  - View button opens PDF in side panel or modal
  - Download button for saving

**Reviewer Verification Checklist - Prerequisites:**
- ☐ Certificate appears legitimate (has OTI center information)
- ☐ Completion date is within 7-year validity window
- ☐ Certificate issued by authorized OTI Education Center
- ☐ Course number matches requirement
- ☐ Applicant name on certificate matches application name
- ☐ No signs of alteration or forgery
- Notes field
- For non-USF certificates: Option to flag for phone/email verification with issuing center

**Special Prerequisites:**
(If OSHA #5600 applied for)
- Current Trainer Authorization: Uploaded/Not Uploaded
- HAZWOPER or Union Credentials: Which option selected, document status
- Verification status

**All Supporting Documents:**
Comprehensive list of all uploaded documents:
- Document Type
- Filename
- Upload Date
- File Size
- Download button
- View button
- Flag Document button (if concerns)

**Tab 5: Verification & Checks**

**Watch List Verification:**

**OSHA Outreach Trainer Watch List:**
- External Link: "Check OSHA Watch List" (opens https://www.osha.gov/training/outreach/trainers/watchlist)
- Instructions: "Search for applicant name in OSHA watch list"
- Verification Status: Radio buttons
  - ○ Not Found on OSHA Watch List (clear to proceed)
  - ○ FOUND on OSHA Watch List (requires reinstatement letter)
- If found: Reinstatement Letter section appears
  - Has Reinstatement Letter: Yes/No
  - Upload Reinstatement Letter: File upload (if not already provided)
  - View Letter: Link
  - Letter Reviewed and Approved: Checkbox

**OTI Education Centers Watch List (Internal):**
- Search Application: Auto-search of applicant name/email against internal watch list
- Status: ✓ Not Found / ⚠ FOUND
- If found: Display match details
  - Name Matched
  - Reason for Watch List Entry
  - Date Added
  - Notes from Watch List
  - Reinstatement Status
  - Override checkbox (with required justification field)

**Contract Course Validation:**
(If contract course selected)
- Registration Code Entered: Display code
- Validation Status: ✓ Valid / ✗ Invalid
- Code Validation Details: Associated institution, valid date range
- Override option (with justification)

**Review Checklist:**
Comprehensive checklist for reviewer:
- ☐ All required personal information is complete and accurate
- ☐ Work experience meets minimum years requirement
- ☐ Work experience is relevant to course applied for
- ☐ Education/certification substitution verified (if claimed)
- ☐ All prerequisite courses documented and verified
- ☐ Prerequisite completion dates within 7-year window
- ☐ All certificates appear legitimate
- ☐ Applicant not on OSHA watch list (or has reinstatement letter)
- ☐ Applicant not on internal watch list (or override approved)
- ☐ Contract course registration code validated (if applicable)
- ☐ Application submitted at least 2 weeks before course start
- ☐ All supporting documents are clear and legible

**AI-Assisted Review:**
(Future enhancement placeholder)
- AI Recommendation: Display AI-generated summary
- Confidence Score: Percentage
- Flagged Issues: List of items AI identified for review
- Note: "AI recommendation is advisory only. Reviewer makes final decision."

**Tab 6: History & Comments**

**Status History:**
Timeline of all status changes:
- Timestamp
- Previous Status → New Status
- Changed By: User name
- Comment (if any)

**Reviewer Comments:**
All comments added by reviewers (visible to other reviewers only, not applicants):
- Timestamp
- Reviewer Name
- Comment Type: Internal Note / Discussion
- Comment Text
- Add New Internal Comment: Text area with Save button

**Applicant Communication:**
All comments sent to applicant or received from applicant:
- Timestamp
- Direction: To Applicant / From Applicant
- Sender Name
- Comment Text
- Attached Documents (if any)

**Add Comment to Applicant:**
- Text area for composing message
- Attach Documents: File upload
- Send Comment: Button (sends email notification to applicant)

**Review Actions Panel:**
(Sticky footer or side panel, always visible)

**Action Buttons:**
Three primary action buttons based on review decision:

**1. Approve Application:**
- Large green button
- Click opens confirmation modal:
  - "Are you sure you want to approve this application?"
  - Checklist must be complete (all items checked)
  - Final Comments: Optional text area
  - Confirm Approval button
- On approval:
  - Updates APPLICATION.status to "Approved"
  - Sets APPLICATION.approval_date to current timestamp
  - Sets APPLICATION.approved_by_user_id to current reviewer
  - Generates enrollment link and registration code
  - Signs OSHA Prerequisite Verification PDF as approver
  - Sends approval email to applicant with enrollment instructions
  - Updates application timeline

**2. Return for Information:**
- Yellow button
- Click opens modal:
  - "Request Additional Information from Applicant"
  - Required: Specify what information is needed (text area, min 50 characters)
  - Optional: Attach reference documents or examples
  - Select specific items needing clarification:
    - ☐ Work experience details
    - ☐ Education documentation
    - ☐ Certification verification
    - ☐ Prerequisite course certificates
    - ☐ Other (specify)
  - Send Request button
- On return:
  - Updates APPLICATION.status to "Returned for Information"
  - Creates APPLICATION_COMMENT record
  - Sends email to applicant with reviewer's comments
  - Keeps application assigned to current reviewer
  - Updates timeline

**3. Deny Application:**
- Red button
- Click opens modal:
  - "Deny Application"
  - Warning: "This is a final denial. The applicant will not be able to register for this course."
  - Required: Reason for denial (dropdown + text area)
    - Dropdown options:
      - Did not demonstrate completion of prerequisite course within previous seven years
      - Did not demonstrate required years of experience
      - Did not include required transcripts
      - Did not submit proof of applicable certification or degree
      - Found on watch list without reinstatement
      - Other (explain)
    - Detailed explanation (text area, min 100 characters)
  - Optional: Guidance for reapplication
  - Confirm Denial button (requires typing "DENY" to confirm)
- On denial:
  - Updates APPLICATION.status to "Denied"
  - Sets APPLICATION.denial_date and denial_reason
  - Sets denied_by_user_id to current reviewer
  - Marks OSHA Prerequisite Verification PDF as "Not Approved" and signs
  - Sends denial email to applicant with reason
  - Updates timeline

**Additional Actions:**
- **Save Progress:** Saves reviewer's checklist progress and notes without changing application status
- **Assign to Another Reviewer:** Transfer application to different reviewer
- **Flag for Discussion:** Marks application for team review/discussion
- **Print Review Summary:** Generates printable summary of review

**Navigation Shortcuts:**
- Previous Application: Navigate to previous in queue
- Next Application: Navigate to next in queue
- Return to Queue: Back to Applications Queue

#### Available Actions and Buttons
[Covered in Review Actions Panel above]

#### Validation Rules
- To approve: All checklist items must be checked
- To return for information: Comment must be at least 50 characters
- To deny: Must select reason and provide detailed explanation (min 100 characters)
- To deny: Must type "DENY" in confirmation field
- Cannot approve if on watch list without reinstatement letter
- Cannot approve if prerequisite courses expired (> 7 years)
- Cannot approve if insufficient qualifying experience
- System validates 2-week deadline before approval

#### Navigation
- **How users access this screen:**
  - Click application number from Applications Queue
  - "Review" action from Applications Queue
  - "Review Now" or "Review Next" from Approver Dashboard
  - Direct link from email notifications
- **Where users can go from here:**
  - Previous/Next application in queue
  - Applications Queue (return to list)
  - Watch List Verification Screen
  - External verification websites (open in new tab)

#### Related Data and Relationships
- Displays complete APPLICATION entity record
- Related to APPLICANT entity (displays applicant information)
- Related to WORK_EXPERIENCE entities (displays all employers)
- Related to EDUCATION entity (displays degree information)
- Related to CERTIFICATION entities (displays all certifications)
- Related to PREREQUISITE_COURSE entities (displays all prerequisite courses)
- Related to SUPPORTING_DOCUMENT entities (displays all documents)
- Related to APPLICATION_COURSE entities (displays courses applied for)
- Related to COURSE entities (displays course details)
- Related to WATCH_LIST entities (checks for watch list matches)
- Related to APPLICATION_COMMENT entities (displays and creates comments)
- Updates APPLICATION entity on approval/denial/return actions

#### Requirements Source
"The approver will have three distinct actions they can take on any application: Approve: Finalizes the application and notifies the student. Return for Information: A 'soft denial' that sends the application back to the applicant with comments if information is missing or unclear. Deny: A 'hard denial' for applicants who are definitively ineligible" (Application Review, line 74-77)

"The reviewer will go to other linked websites to verify that the applicant is not on a public watch list. They will also check against an internal watch list" (Application Review, line 78)

"The reviewer will check claimed certifications and prerequisite courses in external websites that are accessible from the Appian interface" (Application Review, line 80)

"Verifying transcripts is a visual verification of the uploaded transcript the Applicant included in the submission to confirm it has a seal and signature" (Application Review, line 80)

"For the subjective/judgment part of the review, the reviewer must manually read the long-form responses and determine whether they satisfy the criteria" (Application Review, line 82)

"Reviewers would like the ability to compare details across application versions. In the event that an application is sent back, they should be able to see what's been changed" (Auditability, line 185)

#### Priority
Critical - Core review workflow screen, most complex and frequently used by approvers

---


### Screen 13: Watch List Verification Screen

#### Screen Type
Interface (Verification Tool)

#### Target Persona
Approvers

#### Purpose and Business Context
Dedicated interface for verifying applicants against both external OSHA watch lists and internal OTI Education Centers watch list. Streamlines the verification process by consolidating both watch list checks in one place with direct links to external systems. Documents verification results for audit purposes.

#### Interface Parameters
- **applicationId** (integer) - ID of application being verified
- **applicantName** (text) - Applicant's legal name pre-filled for search

#### Data and Fields Displayed

**Applicant Information:**
- Legal Name: Display from application
- Email: Display from application
- Company: Display from application
- Application Number: Link back to Application Review Interface
- Courses Applied For: List

**OSHA Outreach Trainer Watch List (External)**

**Section Header:** "Official OSHA Watch List Verification"

**Instructions:**
"Check if the applicant appears on the official OSHA Outreach Training Program Watch List. This list contains suspended or restricted trainers."

**External Link:**
- Large button: "Open OSHA Watch List" (opens https://www.osha.gov/training/outreach/trainers/watchlist in new tab)
- Link opens in new browser tab/window
- Instructions: "Search for: [Applicant Name]"
- Helpful tip: "Try variations of the name if exact match not found (e.g., with/without middle name)"

**Verification Results:**
Radio button options:
- ○ NOT FOUND on OSHA Watch List
  - Sub-text: "Applicant is clear to proceed"
  - Verification Date: Auto-filled with current timestamp
  - Verified By: Current reviewer name

- ○ FOUND on OSHA Watch List
  - Sub-text: "Applicant is on the official OSHA watch list. Reinstatement letter is REQUIRED."
  - Watch List Details Fields (appear when selected):
    - Reason on Watch List: Text area (copy from OSHA website)
    - Date Added to Watch List: Date picker
    - Watch List Entry Notes: Text area for additional details
  - Reinstatement Status: Radio buttons
    - ○ Reinstatement Letter Provided and Approved
      - Upload Reinstatement Letter: File upload (if not already attached)
      - View Reinstatement Letter: Link to document
      - Letter Approval Date: Date picker
      - Approved By: Text (OSHA official who signed)
    - ○ No Reinstatement Letter Provided
      - Action: "Application must be DENIED or RETURNED for reinstatement letter"

**OTI Education Centers Watch List (Internal)**

**Section Header:** "Internal OTI Watch List Verification"

**Instructions:**
"Check if the applicant appears on the internal watch list maintained by OTI Education Centers. This list is shared among centers and includes problematic applicants who may not be official trainers."

**Automatic Search:**
- System automatically searches internal WATCH_LIST table on page load
- Search by: Applicant name (full and partial matches), email address
- Search Results Display:
  - Status: "Searching..." → "Search Complete"

**Search Results:**

**If NO MATCH FOUND:**
- Display: ✓ "Not Found on Internal Watch List"
- Message: "No matches found. Applicant is clear from internal watch list perspective."
- Verification Date: Auto-filled timestamp
- Verified By: Current reviewer name

**If MATCH FOUND:**
- Display: ⚠ "FOUND on Internal Watch List"
- Match Details:
  - Name Matched: Display matching name from watch list
  - Email Matched: Display matching email (if applicable)
  - Match Confidence: Exact / Likely / Possible
  - Date Added to Watch List: From WATCH_LIST.date_added
  - Added By: From WATCH_LIST.added_by (which OTI center)
  - Reason for Watch List: From WATCH_LIST.reason (display full text)
  - Notes: From WATCH_LIST.notes
  - Reinstatement Status: From WATCH_LIST.reinstated (Yes/No)
  - Reinstatement Date: If applicable

**Reviewer Action Required:**
(If found on internal watch list)
Radio button options:
- ○ Confirm Match and DENY Application
  - Reason will be recorded as watch list entry
- ○ Confirm Match but OVERRIDE (Allow to Proceed)
  - Required: Override Justification (text area, min 100 characters)
  - Required: Manager Approval checkbox (if policy requires)
  - Override will be logged for audit
- ○ False Positive (Not the Same Person)
  - Required: Explanation (text area)
  - Applicant will be cleared

**Manual Search Option:**
- Search by Different Name: Text input to try alternative name spellings
- Search by Email: Text input to search by email only
- Search by Company: Text input to search by employer name
- Re-run Search: Button

**Verification Summary:**

**Overall Verification Status:**
- OSHA Watch List: ✓ Clear / ⚠ Found (status)
- Internal Watch List: ✓ Clear / ⚠ Found (status)
- Overall Determination:
  - ✓ APPROVED for Registration (both clear or appropriately resolved)
  - ⚠ REQUIRES ATTENTION (found with no resolution)
  - ✗ MUST BE DENIED (found without reinstatement/override)

**Audit Trail:**
- Verification Performed By: Current reviewer
- Verification Date/Time: Timestamp
- OSHA Watch List Checked: Yes/No with timestamp
- Internal Watch List Checked: Yes/No with timestamp
- Results: Summary text
- Actions Taken: Log of any overrides or special handling

#### Available Actions and Buttons
- **Save Verification Results:** Saves verification status and details to application record
  - Updates APPLICATION.watch_list_verification_status
  - Creates VERIFICATION_LOG record for audit
  - Returns to Application Review Interface
- **Open OSHA Watch List:** Opens external OSHA website in new tab
- **Re-run Internal Search:** Re-executes search against internal watch list
- **Add to Internal Watch List:** If applicant should be added (separate action, requires justification)
- **Attach Reinstatement Letter:** Upload document if not already provided
- **Request Reinstatement Letter:** Sends email to applicant requesting letter
  - Pre-formatted email template
  - Changes application status to "Returned for Information"
- **Return to Application Review:** Cancels verification and returns without saving
- **Print Verification Report:** Generates printable verification summary

#### Validation Rules
- Must select verification result for OSHA watch list (Found or Not Found)
- Internal watch list search must be performed (automatic but must complete)
- If found on OSHA watch list without reinstatement: Must either deny or return for information
- If found on internal watch list: Must select action (Deny, Override, or False Positive)
- Override requires justification of at least 100 characters
- Cannot mark as "Approved for Registration" if watch list issues are unresolved
- Verification results must be saved before approving application

#### Navigation
- **How users access this screen:**
  - "Check Watch Lists" link from Approver Dashboard
  - "Verify Watch List" action from Application Review Interface
  - Verification tab in Application Review Interface
  - Prompted automatically when reviewing application flagged with watch list concerns
- **Where users can go from here:**
  - Save and Return to Application Review Interface
  - Cancel and Return to Application Review Interface
  - Open OSHA Watch List (external site in new tab)
  - Request Reinstatement Letter (triggers email, returns to application)

#### Related Data and Relationships
- Reads from APPLICATION and APPLICANT entities
- Searches WATCH_LIST entity (internal watch list)
- Updates APPLICATION.watch_list_verification_status
- Creates VERIFICATION_LOG entity record for audit trail
- May create APPLICATION_COMMENT if requesting reinstatement letter
- Links to SUPPORTING_DOCUMENT for reinstatement letters

#### Requirements Source
"The reviewer will go to other linked websites to verify that the applicant is not on a public watch list. They will also check against an internal watch list, which is maintained in a spreadsheet that will be uploaded into Appian on a monthly basis" (Application Review, line 78)

"A specific sub-process will be required for applicants found on a watch list. They can only be approved if they provide an official 'reinstatement letter' from OSHA" (Application Review, line 79)

"Two Distinct Watch Lists: Verification requires checking against two separate lists, not just one: The OSHA Outreach Trainer Watch List: An official, web-based OSHA list of suspended trainers. The OTI Education Centers Watch List: A manually created and shared spreadsheet among all OTI centers listing problematic applicants" (Integration Notes, line 127-129)

#### Priority
High - Critical compliance check, required for all applications before approval

---

### Screen 14: Administrator Dashboard

#### Screen Type
Landing Page

#### Target Persona
Administrators

#### Purpose and Business Context
Main entry point for administrators responsible for managing reference data and system configuration. Provides oversight of watch list management, course data imports, and system health metrics. Enables administrators to maintain the operational data that supports the application review process.

#### Interface Parameters
N/A (landing page for Administrator persona)

#### Data and Fields Displayed

**Welcome Section:**
- Administrator Name: Greeting with user's name
- Last Login: Date/time
- System Date/Time: Current timestamp

**Administrative Tasks Summary:**

**Watch List Management:**
- Last Watch List Update: Date of most recent watch list import
- Days Since Last Update: Calculated countdown
- Update Status: ✓ Current (< 30 days) / ⚠ Update Due Soon (25-30 days) / ✗ Overdue (> 30 days)
- Watch List Entries Count: Total number of entries in internal watch list
- Quick Action: "Update Watch List Now" button

**Course Data Management:**
- Last Course Data Import: Date of most recent course import
- Next Expected Import: Calculated date (monthly cycle)
- Active Courses Count: Number of courses with future start dates
- Upcoming Courses (Next 90 Days): Count
- Quick Action: "Import Course Data" button

**System Health Metrics:**

**Application Volume:**
- Applications This Month: Count
- Applications Pending Review: Count with link to queue
- Applications with Watch List Flags: Count requiring attention
- Applications Approaching Deadline: Count of applications with course start < 2 weeks

**User Activity:**
- Active Applicants (Last 30 Days): Count
- Active Reviewers: Count
- Average Review Time: Days/hours

**Recent Administrative Actions:**
Activity feed of last 15 admin actions:
- Timestamp
- Admin User Name
- Action Type: (Watch List Updated, Course Data Imported, Configuration Changed, User Added, etc.)
- Details: Brief description
- Status: Success/Failed

**Scheduled Maintenance Reminders:**
- Monthly Watch List Update: Next due date with countdown
- Monthly Course Data Import: Next due date with countdown
- Quarterly System Review: Next due date
- Annual Data Archival: Next due date

**Quick Actions Panel:**
- Update Watch List: Navigate to Watch List Management Interface
- Import Course Data: Navigate to Course Data Import Interface
- View All Applications: Navigate to Applications Queue (admin view)
- Manage Users: Link to user management (if applicable)
- System Reports: Link to reporting dashboard
- System Configuration: Link to settings (if applicable)

**Alerts and Notifications:**
- Critical Alerts: Red banner for urgent issues
  - Watch list overdue
  - Course data import failed
  - System errors
- Warnings: Yellow banner for attention needed
  - Watch list update due soon
  - Disk space warnings
  - Performance issues
- Informational: Blue banner for notices
  - Scheduled maintenance
  - New features
  - System updates

**Help and Resources:**
- Administrator Guide: Link to documentation
- Watch List Update Procedure: Step-by-step guide
- Course Data Format Specification: Link to data format documentation
- Contact IT Support: Email/phone for technical issues

#### Available Actions and Buttons
- **Update Watch List:** Opens Watch List Management Interface
- **Import Course Data:** Opens Course Data Import Interface
- **View Applications Queue:** Opens full application list with admin privileges
- **Generate System Report:** Creates comprehensive system status report
- **Dismiss Alert:** Acknowledges and clears alerts from dashboard
- **Configure Reminders:** Customizes reminder schedule and recipients

#### Validation Rules
N/A (informational dashboard)

#### Navigation
- **How users access this screen:**
  - Default landing page after login for Administrator persona
  - Accessible via site navigation "Admin Dashboard" or "Home" link
- **Where users can go from here:**
  - Watch List Management Interface
  - Course Data Import Interface
  - Applications Queue (admin view)
  - User management screens
  - System configuration screens
  - Reporting dashboards

#### Related Data and Relationships
- Reads WATCH_LIST entity for watch list statistics
- Reads COURSE entity for course data statistics
- Reads APPLICATION entity for application volume metrics
- Reads system audit logs for recent administrative actions
- Tracks WATCH_LIST.last_updated_date for update status
- Tracks COURSE.last_imported_date for import status

#### Requirements Source
"Administrators: Application admins that will be able to manage reference data (such as internal watch list)" (Key Actors, line 55)

"Watch List Management: On a monthly basis, the updated watch lists will need to be imported into Appian for use in this process. The application administrators will have access to do this update either through spreadsheet import or manual actions to update one-off" (Watch List Management, line 83)

"We will create email reminders to the admin persona (once per month) to upload the current version of the list" (Application Review, line 78)

"Approximately once a month, 12 future months of course data will be imported into Appian" (Notable Business Rules, line 93)

#### Priority
Medium - Important for system administration but not primary workflow

---

### Screen 15: Watch List Management Interface

#### Screen Type
Interface (Data Management Form)

#### Target Persona
Administrators

#### Purpose and Business Context
Allows administrators to maintain the internal OTI Education Centers watch list through either bulk import from spreadsheet or individual entry/editing. Supports monthly watch list updates from shared spreadsheet among OTI centers. Provides audit trail of all watch list changes for compliance purposes.

#### Interface Parameters
N/A (administrative data management interface)

#### Data and Fields Displayed

**Page Header:**
- Title: "Internal Watch List Management"
- Last Update: Date of most recent watch list change
- Total Entries: Count of watch list entries
- Active Entries: Count of non-reinstated entries

**Import Methods Tabs:**
- Tab 1: Bulk Import from Spreadsheet
- Tab 2: Manual Entry/Edit
- Tab 3: Watch List Search and Review

**Tab 1: Bulk Import from Spreadsheet**

**Instructions:**
"Upload the monthly watch list spreadsheet received from OTI Education Centers. The spreadsheet will be validated and imported into the system. Existing entries will be updated and new entries will be added."

**Import Section:**

**Upload File:**
- File Upload Component: Accepts Excel (.xlsx, .xls) or CSV (.csv)
- File Size Limit: 5MB
- Template Download: "Download Watch List Template" link
  - Provides correctly formatted template for data entry
- Help Text: "File must contain columns: Name, Email, Organization, Reason, Date Added, Status, Notes"

**File Preview:**
(After file selected, before import)
- Display first 10 rows of uploaded file in table format
- Column Mapping:
  - System validates required columns exist
  - Allows mapping if column names don't match exactly
  - Required columns: Name, Reason, Date Added
  - Optional columns: Email, Organization, Status, Notes
- Data Validation Summary:
  - Total Rows: Count
  - Valid Rows: Count (green)
  - Rows with Errors: Count (red) with error details
  - New Entries: Count to be added
  - Updates to Existing: Count to be updated
  - Duplicates: Count (flagged for review)

**Validation Errors Panel:**
(If errors found)
- List of rows with errors:
  - Row Number
  - Error Type: (Missing required field, Invalid date format, Duplicate entry, etc.)
  - Error Details: Specific issue description
  - Action: Skip row or fix manually

**Import Options:**
- Radio buttons:
  - ○ Add new entries and update existing entries (recommended)
  - ○ Replace entire watch list (caution: deletes all current entries)
  - ○ Add new entries only (keep all existing, don't update)
- Checkboxes:
  - ☐ Send notification email when import completes
  - ☐ Create audit report of changes

**Import Actions:**
- Validate File: Button to run validation checks before importing
- Import Watch List: Button to execute import (enabled after validation passes)
- Cancel: Discards upload and returns

**Import Results:**
(After import completes)
- Success banner: "Watch List import completed successfully"
- Import Summary:
  - New Entries Added: Count
  - Existing Entries Updated: Count
  - Entries Unchanged: Count
  - Errors Encountered: Count with details
  - Import Date/Time: Timestamp
  - Imported By: Administrator name
- View Audit Report: Link to detailed change log
- Download Import Log: CSV file of all changes

**Tab 2: Manual Entry/Edit**

**Add New Entry Section:**

**Entry Form:**
- Name: Text input (required)
  - Help text: "Full legal name of person to add to watch list"
- Email: Text input with email validation (optional)
- Organization/Company: Text input (optional)
- Reason for Watch List: Dropdown (required)
  - Options:
    - Fraudulent Documentation
    - Falsified Credentials
    - Misconduct
    - OSHA Suspension
    - Certification Revoked
    - Other (explain)
- Detailed Reason: Text area (required)
  - Character limit: 500
  - Help text: "Provide specific details about why this person is on the watch list"
- Date Added: Date picker (required, defaults to today)
- Added By: Text input (defaults to current admin user)
- Source: Dropdown
  - Internal USF
  - Other OTI Education Center (specify)
  - OSHA
  - Other (specify)
- Status: Dropdown
  - Active (default)
  - Reinstated
  - Under Review
- Reinstatement Date: Date picker (optional, enabled if status = Reinstated)
- Notes: Text area (optional)
  - Character limit: 1000
  - Help text: "Additional context, follow-up actions, or updates"

**Actions:**
- Add to Watch List: Button (validates and saves new entry)
- Clear Form: Resets all fields
- Cancel: Returns without saving

**Search Existing Entries:**

**Search Filters:**
- Name Search: Text input with auto-suggest
- Email Search: Text input
- Organization Search: Text input
- Status Filter: Dropdown (All / Active / Reinstated / Under Review)
- Date Range: Date pickers (Added between dates)
- Search Button: Executes search
- Clear Filters: Resets all filters

**Search Results Grid:**
Columns:
- Name: Sortable
- Email: Sortable
- Organization: Sortable
- Reason: Abbreviated with tooltip for full text
- Date Added: Sortable
- Status: Badge with color coding
- Added By: Text
- Actions: Row menu
  - Edit
  - Mark as Reinstated
  - Delete
  - View Full Details

**Edit Entry Modal:**
(Opens when Edit action selected)
- Display all fields from Add New Entry form
- Pre-populated with current values
- Allow editing all fields
- History section showing:
  - Created Date and Created By
  - Last Modified Date and Modified By
  - Change Log: List of previous modifications
- Update Button: Saves changes
- Delete Button: Removes entry with confirmation
- Cancel Button: Closes without saving

**Tab 3: Watch List Search and Review**

**Quick Verification Tool:**
- Search Name: Large text input
  - "Enter name to check against watch list"
- Search Button: Executes search
- Results:
  - NOT FOUND: ✓ "No matches found in watch list"
  - FOUND: ⚠ "Match found" with full details displayed
  - POSSIBLE MATCH: "Similar names found - review needed"

**Recent Watch List Activity:**
- Last 20 watch list checks performed during application reviews:
  - Timestamp
  - Applicant Name Searched
  - Reviewer
  - Result: Found / Not Found
  - Action Taken: (if found)
  - Application Number: Link

**Watch List Statistics:**
- Total Entries: Count
- Entries by Status:
  - Active: Count
  - Reinstated: Count
  - Under Review: Count
- Entries by Reason:
  - Pie chart or bar graph showing distribution
- Entries Added This Month: Count
- Entries Added This Year: Count
- Most Common Reasons: Top 5 list

**Export Options:**
- Export Full Watch List: Button
  - Format: Excel or CSV
  - Includes all fields and current status
  - Timestamped filename
- Export Filtered Results: Button (if search/filters applied)
- Export Audit Log: Button
  - All changes to watch list in date range

#### Available Actions and Buttons
[Covered in tabs above, including:]
- Bulk Import: Validate File, Import Watch List
- Manual Entry: Add to Watch List, Update Entry, Delete Entry
- Search and Review: Search, Export, View Details

#### Validation Rules
**For Bulk Import:**
- File must be .xlsx, .xls, or .csv format
- Required columns must be present: Name, Reason, Date Added
- Name field cannot be empty
- Date Added must be valid date format
- Email must be valid email format if provided
- No duplicate entries within import file

**For Manual Entry:**
- Name is required (minimum 2 characters)
- Reason dropdown must be selected
- Detailed Reason must be at least 20 characters
- Date Added is required and cannot be future date
- Email must be valid format if provided
- Reinstatement Date required if Status = Reinstated
- Reinstatement Date must be after Date Added

**For Edit/Delete:**
- Cannot delete entry if currently referenced in active application review
- Confirmation required for deletion
- Must provide reason for status change to Reinstated

#### Navigation
- **How users access this screen:**
  - "Update Watch List" from Administrator Dashboard
  - Navigation menu "Watch List Management"
  - Email reminder link (monthly notification)
  - Direct link from application review if watch list issue encountered
- **Where users can go from here:**
  - Administrator Dashboard (return/back)
  - Application Review Interface (if reviewing specific applicant)
  - Audit reports

#### Related Data and Relationships
- Creates, reads, updates, deletes WATCH_LIST entity records
- Creates WATCH_LIST_AUDIT_LOG entity records for all changes
- Related to USER entity (tracks who added/modified entries)
- Related to APPLICATION entity (links to applications that triggered watch list flags)
- Import process may update multiple WATCH_LIST records in bulk transaction

#### Requirements Source
"Watch List Management: On a monthly basis, the updated watch lists will need to be imported into Appian for use in this process. The application administrators will have access to do this update either through spreadsheet import or manual actions to update one-off" (Watch List Management, line 83)

"The OTI Education Centers Watch List: A manually created and shared spreadsheet among all OTI centers listing problematic applicants who may not be official trainers" (Integration Notes, line 129)

"We will create email reminders to the admin persona (once per month) to upload the current version of the list" (Application Review, line 78)

#### Priority
High - Critical administrative function, required monthly, impacts application approvals

---


### Screen 16: Course Data Import Interface

#### Screen Type
Interface (Data Management Form)

#### Target Persona
Administrators

#### Purpose and Business Context
Allows administrators to import course data from Enrole/Informer system on a monthly basis. Approximately 12 months of future course data is imported to keep the system current with available course offerings. Supports both CSV import and potential API integration.

#### Interface Parameters
N/A (administrative data import interface)

#### Data and Fields Displayed

**Page Header:**
- Title: "Course Data Import"
- Last Import: Date and time of most recent successful import
- Next Scheduled Import: Expected date of next monthly import
- Active Courses: Count of courses with future start dates

**Import Method Selection:**
Radio buttons:
- ○ Import from CSV File
- ○ Import via API (if available)
- ○ Manual Course Entry

**Import from CSV File Section:**
(Default selected)

**Instructions:**
"Upload the monthly course data export from Enrole/Informer. The file should contain all courses for the next 12 months. Existing courses will be updated and new courses will be added."

**Upload File:**
- File Upload Component: Accepts CSV (.csv) or Excel (.xlsx, .xls)
- File Size Limit: 10MB
- Template Download: "Download Course Import Template" link
- Help Text: "File must contain columns: Course Number, Course Name, Start Date, End Date, Location, Instructor, Is Contract Course, Registration Code Required, Capacity, Status"

**File Preview:**
(After file selection)
- Display first 10 rows in table format
- Column Mapping Tool:
  - Auto-detect column mappings
  - Allow manual mapping if needed
  - System Fields → File Columns:
    - course_number → [dropdown to select column]
    - course_name → [dropdown]
    - start_date → [dropdown]
    - end_date → [dropdown]
    - location → [dropdown]
    - instructor_name → [dropdown]
    - is_contract → [dropdown]
    - registration_code_required → [dropdown]
    - max_capacity → [dropdown]
    - current_enrollment → [dropdown]
    - status → [dropdown]

**Data Validation Summary:**
- Total Rows in File: Count
- Valid Rows: Count (green checkmark)
- Rows with Errors: Count (red X) with expandable error details
- New Courses to Add: Count
- Existing Courses to Update: Count
- Courses to Deactivate: Count (courses in system but not in import file)
- Duplicate Entries: Count with details

**Validation Error Details:**
(If errors found)
Table showing:
- Row Number in File
- Course Number
- Error Type:
  - Missing required field
  - Invalid date format
  - Start date after end date
  - Duplicate course number
  - Invalid course number format
  - Past start date (warning only)
- Error Description
- Action: Dropdown (Skip Row / Fix Manually / Import Anyway)

**Import Options:**
Checkboxes:
- ☐ Update existing course information (recommended)
- ☐ Add new courses
- ☐ Deactivate courses not in import file (courses that were removed from schedule)
- ☐ Preserve manual edits to courses (don't overwrite certain fields like notes)
- ☐ Send notification email when import completes
- ☐ Generate import report

**Date Range Validation:**
- Earliest Course Date in File: Date
- Latest Course Date in File: Date
- Expected Range: Current month + 12 months
- Warning if dates outside expected range

**Import Actions:**
- Validate Import File: Button to run validation without importing
- Preview Changes: Button to show side-by-side comparison of changes
- Import Courses: Primary button (enabled after validation passes)
- Cancel: Discards upload

**Import Results:**
(After import completes)
- Success Message: "Course data import completed successfully"
- Import Summary:
  - New Courses Added: Count with list
  - Courses Updated: Count with list
  - Courses Deactivated: Count with list
  - Errors Encountered: Count with details
  - Import Date/Time: Timestamp
  - Imported By: Administrator name
  - Processing Time: Duration
- Download Import Report: PDF or CSV with full details
- View Change Log: Link to audit trail

**Import via API Section:**
(If API integration available)

**API Connection Status:**
- Connection Status: ● Connected / ● Disconnected
- Last Successful Sync: Date/time
- API Endpoint: Display URL
- Test Connection: Button

**API Import Options:**
- Sync Interval: Dropdown
  - Manual (on-demand)
  - Daily
  - Weekly
  - Monthly
- Date Range: Date pickers (import courses between dates)
- Course Types: Checkboxes (select which course types to import)
  - ☐ Trainer Courses (#500, #501, #5400, #5600)
  - ☐ Update Courses (#502, #503, #5402, #5602)
  - ☐ Contract Courses
  - ☐ All Courses

**API Import Actions:**
- Sync Now: Button to trigger immediate API sync
- Schedule Sync: Opens dialog to configure automated sync
- View Sync History: Shows log of previous API syncs

**Manual Course Entry Section:**

**Add New Course Form:**
- Course Number: Dropdown or text input (required)
  - Common options: OSHA #500, #501, #502, #503, #5400, #5402, #5600, #5602
  - Or: Custom text entry
- Course Name: Text input (required)
  - Auto-populated based on course number selection
  - Editable for custom courses
- Start Date: Date picker (required)
- End Date: Date picker (required)
  - Validation: Must be after start date
- Location: Text input (City, State format) (required)
- Instructor Name: Text input (optional)
- Course Type: Dropdown (required)
  - Trainer Course
  - Update Course
  - Special Course
- Is Contract Course: Checkbox
  - If checked: Registration code setup fields appear
- Registration Code Required: Checkbox
  - If checked: Registration codes can be entered
- Registration Codes: Multi-entry text area
  - One code per line
  - Each code can be used by specific organization's employees
- Maximum Capacity: Number input (optional)
- Current Enrollment: Number input (optional, defaults to 0)
- Course Status: Dropdown (required)
  - Active (available for applications)
  - Draft (not yet published)
  - Cancelled
  - Completed
- Course Description: Text area (optional)
  - Rich text editor for formatting
- Prerequisites: Multi-select checkboxes
  - Auto-populated based on course number
  - Can be customized
- Notes: Text area (optional)
  - Internal notes for administrators
- Is Visible to Applicants: Checkbox (default: checked)

**Actions:**
- Add Course: Button (validates and saves)
- Clear Form: Resets all fields
- Cancel: Returns without saving

**Course List and Management:**

**Search and Filter:**
- Search: Text input (search by course number, name, location)
- Date Range Filter: Date pickers (courses between dates)
- Status Filter: Dropdown (All / Active / Draft / Cancelled / Completed)
- Course Type Filter: Dropdown (All / Trainer / Update / Contract)
- Location Filter: Dropdown of all locations

**Courses Grid:**
Columns:
- Course Number: Sortable
- Course Name: Sortable
- Start Date: Sortable
- End Date: Sortable
- Location: Sortable, filterable
- Type: Badge (Trainer / Update / Contract)
- Status: Badge with color coding
- Capacity: X / Y (enrolled / max)
- Applications: Count of applications for this course
- Actions: Row menu
  - Edit
  - Duplicate (create copy)
  - Cancel Course
  - View Applications
  - Delete

**Grid Features:**
- Sortable columns
- Pagination: 50 per page
- Export to CSV
- Bulk actions (select multiple courses):
  - Change status
  - Cancel selected courses
  - Export selected

**Edit Course Modal:**
(Opens when Edit selected)
- Same fields as Add New Course Form
- Pre-populated with current values
- Shows:
  - Created Date and By
  - Last Modified Date and By
  - Application Count (how many applications for this course)
  - Enrolled Applicants (if approved)
- Update Course: Button to save changes
- Delete Course: Button (with confirmation, only if no applications)
- Cancel: Close without saving

#### Available Actions and Buttons
[Covered in sections above, including:]
- CSV Import: Validate, Preview, Import
- API Sync: Test Connection, Sync Now, Schedule Sync
- Manual Entry: Add Course, Edit Course, Delete Course
- Export: Download Template, Download Import Report, Export Course List

#### Validation Rules
**For CSV Import:**
- File must be .csv, .xlsx, or .xls format
- Required fields: Course Number, Course Name, Start Date, End Date, Location, Status
- Course Number must follow OSHA format (e.g., #500, #501)
- Start Date must be valid date format
- End Date must be valid date and after Start Date
- Start Date should be in future (warning if past, doesn't block import)
- Is Contract should be boolean (true/false, yes/no, 1/0)
- Capacity must be positive integer if provided

**For Manual Entry:**
- Course Number is required
- Course Name is required (minimum 5 characters)
- Start Date is required and should be current or future date
- End Date is required and must be after Start Date
- Location is required
- Course Status is required
- If Is Contract = true, at least one registration code should be provided
- Capacity must be positive integer
- Current Enrollment cannot exceed Maximum Capacity

**For Edit/Delete:**
- Cannot delete course if applications exist for it (must cancel instead)
- Cancelling course with approved applications requires confirmation and notification
- Changing dates within 2 weeks of course start triggers warning

#### Navigation
- **How users access this screen:**
  - "Import Course Data" from Administrator Dashboard
  - Navigation menu "Course Data Management"
  - Email reminder link (monthly import reminder)
- **Where users can go from here:**
  - Administrator Dashboard (return/back)
  - View Applications for specific course
  - Course edit forms
  - Import reports and audit logs

#### Related Data and Relationships
- Creates, reads, updates COURSE entity records
- Related to APPLICATION_COURSE entity (shows application count per course)
- Creates COURSE_IMPORT_LOG entity for audit trail
- Related to USER entity (tracks who imported/modified courses)
- Validates against COURSE_REGISTRATION_CODE entity for contract courses

#### Requirements Source
"Applicants will be able to select courses based on the courses available in the Courses synced record" (Application Submission, line 60)

"Course data will be imported from Enrole/Informer. TBD whether Appian will obtain data via CSV import or Informer API" (Records section, line 148)

"Approximately once a month, 12 future months of course data will be imported into Appian" (Notable Business Rules, line 93)

"Currently, course data is accessible in USF's Informer system. (informer is the reporting layer over Enrole). This data can be shared with Appian via CSV or possibly an API" (Integrations, line 123-124)

#### Priority
Medium - Important for keeping course offerings current, monthly recurring task

---

### Screen 17: Approved Applicants Report

#### Screen Type
Report

#### Target Persona
Facilitators, Leadership

#### Purpose and Business Context
Provides facilitators with a list of approved applicants for each course to verify enrollment. Allows facilitators to confirm who has been approved to attend their courses before the course start date. Supports course preparation and enrollment verification workflow.

#### Interface Parameters
- **courseId** (integer) - Optional, to show report for specific course
- **dateRange** (date range) - Optional, to filter by course date range

#### Data and Fields Displayed

**Report Header:**
- Report Title: "Approved Applicants Report"
- Generated Date/Time: Timestamp
- Generated By: User name
- Report Parameters: Display applied filters

**Filter Controls:**

**Course Selection:**
- Course Filter: Dropdown or multi-select
  - All Courses
  - Specific course number (e.g., OSHA #500)
  - By course type (Trainer / Update)
  - By location
- Course Date Range: Date pickers (courses starting between dates)
  - Quick select: Next 30 days, Next 90 days, All Future
- Status Filter: Checkboxes
  - ☑ Approved (default selected)
  - ☐ Enrolled (confirmed enrollment in Enrole)
  - ☐ Include Pending Approval (for preview)

**Grouping Options:**
- Group By: Dropdown
  - Course
  - Course Date
  - Location
  - None (flat list)
- Sort By: Dropdown
  - Course Start Date (default)
  - Applicant Name
  - Approval Date
  - Company

**Apply Filters Button:** Refreshes report with selected filters

**Report Content:**

**For Each Course:** (if grouped by course)

**Course Header:**
- Course Number and Name: e.g., "OSHA #500 - Trainer Course in Occupational Safety and Health for the Construction Industry"
- Course Dates: Start Date - End Date
- Location: City, State
- Instructor: Name
- Capacity: X approved / Y maximum capacity
- Enrollment Status: Count enrolled in Enrole vs. approved in Appian

**Approved Applicants Table:**

Columns:
- Applicant Name: From APPLICANT.legal_name, sortable
- Email: From APPLICANT.email
- Phone: From APPLICATION.phone_number
- Company: From APPLICATION.company_name, sortable
- Job Title: From APPLICATION.job_title
- Approval Date: From APPLICATION.approval_date, sortable
- Enrole Registration Status: Badge
  - ✓ Enrolled (confirmed)
  - ⚠ Pending (not yet enrolled)
  - ✗ Not Enrolled
- Registration Code: If applicable, display code
- Application Number: Link to view application details
- Actions: Row menu
  - View Application
  - Download Application PDF
  - Send Enrollment Reminder (if not enrolled)
  - Export Contact Info

**Course Summary Footer:**
- Total Approved: Count
- Currently Enrolled: Count
- Pending Enrollment: Count
- Available Capacity Remaining: Count

**Report Summary:**
(At end of report)
- Total Courses Included: Count
- Total Approved Applicants: Count across all courses
- Total Enrolled: Count
- Pending Enrollment: Count
- Courses at Capacity: Count
- Courses with Available Space: Count

**Export Options Section:**
- Format Selection: Radio buttons
  - ○ PDF (formatted for printing)
  - ○ Excel (editable spreadsheet)
  - ○ CSV (data only)
- Include Options: Checkboxes
  - ☑ Applicant contact information
  - ☑ Application numbers
  - ☐ Approval dates and reviewer names
  - ☐ Course capacity details
  - ☐ Registration codes (if applicable)

#### Available Actions and Buttons
- **Apply Filters:** Refreshes report with current filter selections
- **Clear Filters:** Resets all filters to defaults
- **Export Report:** Downloads report in selected format (PDF/Excel/CSV)
- **Print Report:** Opens print dialog with print-optimized layout
- **Email Report:** Opens email dialog with report as attachment
  - To: Email addresses
  - Subject: Pre-filled with report title and date
  - Body: Customizable message
  - Attachment: Report in selected format
- **Send Enrollment Reminder:** (Per applicant or bulk)
  - Sends email to approved applicants who haven't enrolled yet
  - Includes enrollment link and registration code
  - Tracks reminder sent date
- **View Application:** Opens Application Review Interface (read-only for Facilitators)
- **Download Application PDF:** Downloads signed OSHA Prerequisite Verification Form
- **Schedule Automatic Report:** Opens dialog to set up recurring email delivery
  - Frequency: Daily/Weekly/Monthly
  - Recipients: Email addresses
  - Filter Presets: Save current filters for automated report

#### Validation Rules
N/A (read-only report)

#### Navigation
- **How users access this screen:**
  - Navigation menu "Reports" > "Approved Applicants"
  - "View Approved Applicants" link from Facilitator dashboard
  - Direct link from course details
  - Email link from scheduled reports
- **Where users can go from here:**
  - Application Review Interface (view specific application)
  - Course details
  - Other reports
  - Export downloads

#### Related Data and Relationships
- Reads APPLICATION entity filtered by status = "Approved"
- Joined with APPLICANT entity (displays applicant information)
- Joined with APPLICATION_COURSE entity (links applications to courses)
- Joined with COURSE entity (displays course information)
- May integrate with Enrole data to show enrollment status (future integration)
- Related to USER entity (displays approver information if included)

#### Requirements Source
"Facilitators: Support course instructors, who will need access to reports on approved participants to verify enrollment" (Key Actors, line 53)

"Facilitators can access a list of approved applicants per class" (Reports section, line 157)

#### Priority
Medium - Important for facilitators, needed before each course starts

---

### Screen 18: Application Statistics Dashboard

#### Screen Type
Dashboard

#### Target Persona
Leadership, Administrators, Approvers

#### Purpose and Business Context
Provides leadership with high-level overview of application metrics, trends, and performance indicators. Enables data-driven decisions about staffing, course offerings, and process improvements. Visualizes key metrics through charts and graphs.

#### Interface Parameters
- **dateRange** (date range) - Optional, defaults to current year
- **comparisonPeriod** (date range) - Optional, for year-over-year or period comparison

#### Data and Fields Displayed

**Dashboard Header:**
- Title: "Application Statistics Dashboard"
- Date Range: Display selected date range
- Last Updated: Timestamp of data refresh
- Refresh Button: Manually refresh data

**Date Range Selector:**
- Quick Select Buttons:
  - Current Month
  - Current Quarter
  - Current Year
  - Last 12 Months
  - Custom Date Range (opens date pickers)
- Comparison Toggle: Checkbox to enable period comparison

**Key Performance Indicators (KPIs):**

**Card Layout - Top Row:**

**Total Applications:**
- Large number display
- Comparison to previous period: ↑ X% or ↓ X%
- Sparkline graph showing trend over time

**Approval Rate:**
- Percentage display with large font
- Color coding: Green if >80%, Yellow if 60-80%, Red if <60%
- Comparison to previous period
- Sub-metrics:
  - Approved: Count (percentage)
  - Denied: Count (percentage)
  - Returned for Info: Count (percentage)

**Average Review Time:**
- Days and hours display
- Comparison to previous period
- Target benchmark line
- Color coding based on meeting target

**Applications Pending:**
- Current count
- Breakdown:
  - Submitted: Count
  - Under Review: Count
  - Returned (awaiting resubmission): Count
- Aging indicator: Count older than 7 days

**Application Volume Trends:**

**Applications Over Time Chart:**
- Line graph showing application submissions by month/week/day
- X-axis: Time periods
- Y-axis: Application count
- Multiple lines:
  - Total Submitted (blue)
  - Approved (green)
  - Denied (red)
  - Returned for Info (yellow)
- Hover tooltip: Shows exact values
- Zoom and pan controls

**Application Status Distribution:**
- Pie chart or donut chart
- Segments:
  - Submitted: % and count
  - Under Review: % and count
  - Returned for Info: % and count
  - Approved: % and count
  - Denied: % and count
- Legend with color coding
- Click segment to drill down

**Course Analysis:**

**Applications by Course Type:**
- Bar chart (horizontal or vertical)
- X-axis: Course numbers (#500, #501, #5400, #5600, updates)
- Y-axis: Application count
- Color coding by course type
- Grouped bars showing:
  - Total applications
  - Approved
  - Denied

**Most Popular Courses:**
- Table or ranked list
- Columns:
  - Course Number and Name
  - Application Count
  - Approval Rate
  - Average Review Time
  - Capacity Utilization: (Approved / Max Capacity)
- Sortable columns

**Courses with Capacity Issues:**
- List of courses where approved applicants exceed or approach capacity
- Warning indicators
- Courses oversubscribed: Red flag
- Courses near capacity: Yellow warning
- Courses undersubscribed: Blue information

**Review Performance:**

**Reviewer Performance Table:**
- Columns:
  - Reviewer Name
  - Applications Reviewed: Count
  - Average Review Time: Days/hours
  - Approval Rate: Percentage
  - Applications Currently Assigned: Count
  - Performance Indicator: Color-coded dot (green/yellow/red)
- Sortable columns
- Filter by reviewer

**Review Time Distribution:**
- Histogram or box plot
- Shows distribution of review times
- Median review time line
- Target review time line
- Outliers highlighted

**Decision Outcomes Analysis:**

**Denial Reasons Breakdown:**
- Bar chart showing counts by denial reason
- Reasons from dropdown used in denials:
  - Insufficient experience
  - Expired prerequisites
  - Missing documentation
  - Watch list issues
  - Other
- Helps identify common issues for applicant education

**Returned for Information Reasons:**
- Similar chart for return reasons
- Identifies areas needing clearer instructions or form improvements

**Demographic and Experience Analysis:**

**Applications by Industry Experience:**
- Pie chart or bar chart
- Based on work experience: Construction, General Industry, Maritime, Other
- Helps understand applicant pool composition

**Education/Certification Substitution Usage:**
- Metric showing:
  - Applicants claiming degree substitution: Count and %
  - Applicants claiming certification substitution: Count and %
  - No substitution: Count and %
- Trend over time

**Geographic Analysis:**

**Applications by Location:**
- Map visualization (if available) OR table
- Shows application volume by state or region
- Color intensity indicates volume
- Click state/region for details

**Course Locations Performance:**
- Table showing:
  - Location (City, State)
  - Number of Courses: Count
  - Total Applications: Count
  - Approval Rate: Percentage
  - Average Class Size: Count

**Watch List Activity:**

**Watch List Matches:**
- Count of applications with watch list flags
- Resolution breakdown:
  - Approved with reinstatement: Count
  - Denied due to watch list: Count
  - False positives: Count
- Trend over time

**Data Quality Indicators:**

**Common Application Issues:**
- Bar chart or table showing:
  - Incomplete work experience: Count
  - Missing documents: Count
  - Expired prerequisites: Count
  - Insufficient experience: Count
  - Contract code issues: Count
- Helps identify user experience improvements needed

**Application Resubmission Rate:**
- Percentage of applications returned and successfully resubmitted
- Average number of resubmission cycles
- Time to resolution after return

**Export and Sharing:**

**Export Options:**
- Export Dashboard as PDF: Full dashboard as printable report
- Export Data to Excel: All underlying data
- Schedule Email Delivery:
  - Frequency: Daily/Weekly/Monthly
  - Recipients: Email list
  - Format: PDF or Excel
  - Automatic generation and send

**Drill-Down Capabilities:**
- Click any metric to see detailed list
- Click chart segments to filter other visualizations
- Filter persistence across navigation
- Reset filters button

#### Available Actions and Buttons
- **Refresh Data:** Reloads all metrics with current data
- **Change Date Range:** Opens date picker dialog
- **Enable Comparison:** Toggles period comparison view
- **Export Dashboard:** Downloads as PDF or Excel
- **Schedule Report:** Sets up automatic email delivery
- **Drill Down:** Click metrics or chart segments for details
- **Reset Filters:** Clears all applied filters
- **Save View:** Saves current filters and settings as named view
- **Share Dashboard:** Generates shareable link or sends email

#### Validation Rules
N/A (read-only dashboard)

#### Navigation
- **How users access this screen:**
  - Navigation menu "Reports" > "Application Statistics"
  - "View Statistics" from Leadership Dashboard
  - "View Metrics" from Approver Dashboard
- **Where users can go from here:**
  - Drill down to detailed reports
  - View specific applications
  - Export data files
  - Other dashboards and reports

#### Related Data and Relationships
- Aggregates data from APPLICATION entity
- Aggregates by APPLICATION.status for status distributions
- Joins with APPLICANT entity for demographic analysis
- Joins with COURSE entity for course analysis
- Joins with USER entity for reviewer performance
- Calculates metrics from APPLICATION timestamps (review times)
- References WATCH_LIST entity for watch list activity

#### Requirements Source
"Leadership: Read-only role with ability to see most or all data in the system" (Key Actors, line 54)

"There is not currently any reporting, but the customer is excited about the opportunity to get the kind of common reports you might expect, like a dashboard showing application numbers, historical application and course enrollment data, and so on" (Reports section, line 154)

#### Priority
Medium - Important for leadership oversight and process improvement

---

### Screen 19: Task Report

#### Screen Type
Report

#### Target Persona
Approvers, Administrators, Leadership

#### Purpose and Business Context
Tracks in-progress applications and provides workload view for approvers. Helps identify bottlenecks, prioritize work, and ensure applications are moving through the review process efficiently. Supports operational management of the review workflow.

#### Interface Parameters
- **assignedTo** (user) - Optional, filter by assigned reviewer
- **statusFilter** (array) - Optional, filter by application status

#### Data and Fields Displayed

**Report Header:**
- Report Title: "Task Report - In-Progress Applications"
- Generated Date/Time: Timestamp
- Report Scope: Display applied filters
- Total Tasks: Count of in-progress applications

**Filter Controls:**

**Status Filter:**
Multi-select checkboxes:
- ☑ Submitted (awaiting review)
- ☑ Under Review (assigned and in progress)
- ☑ Returned for Information (waiting for applicant response)
- ☑ Re-submitted (awaiting re-review)
- ☐ Include Recent Approvals (last 7 days)
- ☐ Include Recent Denials (last 7 days)

**Assignment Filter:**
- Assigned To: Dropdown
  - All Reviewers
  - My Tasks Only (current user)
  - Unassigned
  - Specific Reviewer (select from list)

**Priority Filter:**
- Checkboxes:
  - ☑ Urgent (course starts < 2 weeks)
  - ☑ High Priority (course starts 2-3 weeks)
  - ☑ Normal Priority (course starts > 3 weeks)

**Date Filters:**
- Submitted Date Range: Date pickers
- Course Start Date Range: Date pickers
- In Current Status For: Dropdown
  - All
  - Less than 24 hours
  - 1-3 days
  - 3-7 days
  - More than 7 days (aging)
  - More than 14 days (critical aging)

**Course Filter:**
- Course Number: Multi-select dropdown
- Course Location: Multi-select dropdown

**Apply Filters Button:** Refreshes report

**Task Summary Section:**

**Summary Cards:**

**By Status:**
- Submitted: Count with avg days in status
- Under Review: Count with avg days in status
- Returned for Information: Count with avg days waiting
- Re-submitted: Count ready for re-review

**By Priority:**
- Urgent Tasks: Count (red indicator)
- High Priority: Count (yellow indicator)
- Normal Priority: Count (green indicator)

**By Aging:**
- New (< 24 hours): Count
- Normal (1-7 days): Count
- Aging (7-14 days): Count (yellow warning)
- Critical Aging (> 14 days): Count (red alert)

**Workload Distribution:**
Bar chart showing application count by assigned reviewer
- Helps identify workload imbalance
- Includes unassigned applications

**Task List:**

**Grouped View Options:**
- Group By: Dropdown
  - Status (default)
  - Assigned Reviewer
  - Priority
  - Course
  - None (flat list)
- Sort Within Groups: Dropdown
  - Priority (default)
  - Submission Date (oldest first)
  - Course Start Date (soonest first)
  - Days in Current Status

**Task Table:**

Columns:
- Priority Icon: 🔴 Urgent / 🟡 High / 🟢 Normal
- Application Number: Linked to Application Review Interface
- Applicant Name: From APPLICANT.legal_name
- Company: From APPLICATION.company_name
- Course(s): Badge list of course numbers
- Submission Date: Date, sortable
- Current Status: Badge with color coding
- Days in Status: Count, sortable, color-coded
  - Green: < 3 days
  - Yellow: 3-7 days
  - Orange: 7-14 days
  - Red: > 14 days
- Assigned To: Reviewer name or "Unassigned"
- Course Start Date: Soonest start date from selected courses
- Days Until Course: Countdown, sortable
- Last Activity: Date/time of most recent action
- Next Action Required: Text description
  - "Needs initial review"
  - "Awaiting applicant response"
  - "Ready for re-review"
  - "Verification pending"
- Actions: Row menu
  - Review/Continue Review
  - Assign to Me
  - Reassign
  - Add Note
  - Flag for Attention

**Table Features:**
- Sortable columns
- Multi-select rows for bulk actions
- Expandable rows to see more details
- Row color coding by priority
- Pagination: 50 per page
- Column configuration: Show/hide columns

**Aging Analysis Section:**

**Aging Report Table:**
- Groups applications by time in current status
- Columns:
  - Status
  - Aging Bucket: < 24h, 1-3d, 3-7d, 7-14d, > 14d
  - Count: Number of applications
  - Oldest Application: Days in status
  - Assigned Reviewer: Name
  - Action Required: Recommended next step

**Bottleneck Identification:**
- Highlights stages where applications are getting stuck
- Shows average time in each status
- Compares to target SLA
- Recommendations for process improvement

**Reviewer Task List:**
(For each reviewer if grouped by reviewer)

**Reviewer: [Name]**
- Total Assigned Tasks: Count
- Urgent Tasks: Count
- Average Days in Review: Metric
- Applications Ready for Action: Count
- Applications Waiting on Applicant: Count

**Task List:** (same columns as main task table, filtered by reviewer)

**Export and Actions Section:**

**Bulk Actions:**
(When rows selected)
- Assign Selected To: Dropdown of reviewers
- Flag Selected as Priority
- Add Note to Selected
- Export Selected

**Export Options:**
- Format: PDF / Excel / CSV
- Include: Checkboxes
  - ☑ Summary statistics
  - ☑ Task details
  - ☐ Applicant contact information
  - ☐ Application history
  - ☐ Reviewer notes

#### Available Actions and Buttons
- **Apply Filters:** Refreshes report with selected filters
- **Clear Filters:** Resets to default view
- **Export Report:** Downloads in selected format
- **Print Report:** Opens print dialog
- **Email Report:** Sends report to recipients
- **Review Application:** Opens Application Review Interface
- **Assign Task:** Assigns application to reviewer
- **Reassign Task:** Changes assigned reviewer
- **Flag for Attention:** Marks application for special attention
- **Add Note:** Adds reviewer note to application
- **Send Reminder:** Sends reminder to applicant (if waiting on response)
- **Escalate:** Flags application for management review
- **Schedule Automatic Report:** Sets up recurring email delivery
  - Daily: For daily standup/workload planning
  - Weekly: For weekly team review
  - Customizable filters and recipients

#### Validation Rules
N/A (read-only report)

#### Navigation
- **How users access this screen:**
  - Navigation menu "Reports" > "Task Report"
  - "My Tasks" from Approver Dashboard
  - "View Workload" from Administrator Dashboard
  - Direct link from email notifications
- **Where users can go from here:**
  - Application Review Interface (click application number or Review action)
  - Applications Queue (filtered view)
  - Other reports
  - Export downloads

#### Related Data and Relationships
- Reads APPLICATION entity filtered by in-progress statuses
- Joined with APPLICANT entity (displays applicant information)
- Joined with APPLICATION_COURSE entity (links to courses)
- Joined with COURSE entity (displays course information, calculates priority)
- Related to USER entity (displays assigned reviewer)
- Calculates aging from APPLICATION.submitted_date, APPLICATION.status_change_date
- Related to APPLICATION_COMMENT entity (shows last activity)

#### Requirements Source
"Task Report: Tracks in-progress applications" (Reports section, line 155)

"Approvers: Trained USF employees who review applications and determine whether the Applicant is qualified" (Key Actors, line 51)

"2 week cutoff for applications prior to course start date" (Notable Business Rules, line 88) - indicates need for priority/urgency tracking

#### Priority
High - Critical for daily workflow management and ensuring timely reviews

---

## Navigation Flow Summary

### Applicant Workflow
1. **Self-Registration** (Appian Portal) → Login
2. **Applicant Landing Page** (Screen 1) ↔ **My Applications List** (Screen 9)
3. **Application Wizard** (Screens 2-7):
   - Course Selection → Personal Information → Work Experience → Education & Certifications → Prerequisites & Documents → Review & Sign
4. **Application Status View** (Screen 8) - Monitor progress, respond to requests
5. Loop back to wizard if "Returned for Information"

### Approver Workflow
1. **Approver Dashboard** (Screen 10)
2. **Applications Queue** (Screen 11) - Find applications to review
3. **Application Review Interface** (Screen 12) - Comprehensive review
4. **Watch List Verification Screen** (Screen 13) - Verify applicants
5. Decision: Approve / Return for Information / Deny
6. Return to Applications Queue for next review

### Administrator Workflow
1. **Administrator Dashboard** (Screen 14)
2. **Watch List Management Interface** (Screen 15) - Monthly update
3. **Course Data Import Interface** (Screen 16) - Monthly import
4. Return to Administrator Dashboard

### Facilitator/Leadership Workflow
1. Navigation menu to reports
2. **Approved Applicants Report** (Screen 17) - Verify enrollment
3. **Application Statistics Dashboard** (Screen 18) - View metrics
4. **Task Report** (Screen 19) - Monitor workload

---

## Responsive Design Considerations

### Mobile/Tablet Support
- **Applicant screens (1-9)**: MUST be fully responsive
  - Wizard steps optimized for mobile form completion
  - Touch-friendly file upload
  - Simplified navigation on small screens
  - Language toggle easily accessible
  
- **Approver screens (10-13)**: Desktop-optimized, tablet-friendly
  - Review interface requires larger screen for document viewing
  - Mobile view allows basic queue management and assignment
  
- **Administrator screens (14-16)**: Desktop-optimized
  - Bulk imports require desktop functionality
  - Mobile view for monitoring only

- **Reports (17-19)**: Responsive with prioritized content
  - Charts adapt to screen size
  - Tables become scrollable cards on mobile
  - Export options available on all devices

---

## SAIL Component Recommendations

### For Applicant Screens
- **a!wizardStepBarField()** - Application wizard navigation
- **a!richTextDisplayField()** - Instructions and help text (bilingual)
- **a!fileUploadField()** - Document uploads with preview
- **a!dateField()** - Date pickers with validation
- **a!dropdownField()** - Course selection, states, dropdowns
- **a!textField()** with validation - Personal information entry
- **a!paragraphField()** - Long-form text (work experience descriptions)
- **a!cardLayout()** - Application status cards, course cards
- **a!columnsLayout()** - Responsive form layouts
- **a!sectionLayout()** - Collapsible form sections
- **a!stampField()** - Status badges and priority indicators
- **a!buttonArrayLayout()** - Wizard navigation buttons

### For Approver Screens
- **a!gridField()** - Applications queue grid with sorting/filtering
- **a!sideSheetLayout()** - Quick preview panel
- **a!recordActionField()** - Quick actions from grid
- **a!tabsLayout()** - Application review tabs (Overview, Experience, etc.)
- **a!checkboxField()** - Verification checklists
- **a!richTextDisplayField()** with links - External verification links
- **a!commentField()** - Reviewer comments and communication
- **a!buttonWidget()** with confirmations - Approve/Deny/Return actions
- **a!dynamicLink()** - Navigation within review interface

### For Administrator Screens
- **a!fileUploadField()** with validation - CSV/Excel imports
- **a!gridField()** with editing - Watch list and course management
- **a!recordDataField()** - Data preview before import
- **a!validationMessage()** - Import validation errors
- **a!progressIndicator()** - Import processing status
- **a!pieChartField()** / **a!barChartField()** - Statistics visualization

### For Reports
- **a!gridField()** with export - Data tables
- **a!columnChartField()** / **a!lineChartField()** - Trend analysis
- **a!pieChartField()** - Distribution charts
- **a!cardLayout()** - KPI cards
- **a!kpiField()** - Key metrics display
- **a!dashboardLayout()** - Dashboard grid arrangement
- **a!exportDataStoreEntityToExcel()** - Report exports

---

## Accessibility Considerations

- All applicant screens MUST support English and Spanish
- Color-coded elements must have text labels (not color-only indicators)
- Form fields require clear labels and error messages
- Tab navigation supported throughout
- Screen reader compatible status updates
- High contrast mode support for all screens
- Keyboard shortcuts for frequent actions
- Focus indicators for interactive elements
- Alternative text for all images and icons

---

## End of Screen Specifications Document

