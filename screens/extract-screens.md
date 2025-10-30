# Extract Screens

This prompt will guide the extraction of screen specifications from requirements documents for Appian applications.

## Purpose

To systematically analyze requirements and data models to produce structured screen definitions for Appian SAIL interfaces.

## Input Requirements

### **Source Requirements Document**
Use the requirements document from the `source-requirements/` folder as the primary input for understanding:
- Application purpose and business context
- User workflows and processes
- Screen requirements and user interactions
- Business rules and validation needs

### **Data Model (Optional but Recommended)**
If available, reference the data model YAML file from the `data-model/` folder to:
- Understand available entities and fields
- Ensure screens align with database structure
- Properly reference entity properties and relationships
- Use correct entity names in the "Related Data and Relationships" section

**When data model is NOT available:**
- Infer entity names from requirements
- Use generic entity references (e.g., "CASE entity", "CUSTOMER entity")
- Note assumptions about data structure in screen descriptions

## Instructions
You are a UX planning expert and an expert Appian developer. Generate UX prototype screens given a set of application requirements and an application data model. The goal is to provide the end user with a set of structured, user interface screens for their application.

## CORE PRINCIPLES

### **1. Source Traceability Rule**
- Every screen must trace to a requirement mentioned in the requirements document.
- Quote the requirement for every screen.

### **2. Platform Assumptions**

#### **Appian Site**
An Appian site is a web-based interface or application within the Appian platform that provides a user-friendly way to organize and present content, processes, and data to end users. It serves as the primary entry point for users to interact with Appian applications.

#### **Site Pages**
An Appian site can and typically does have multiple pages. This is one of the key features that makes sites so powerful for organizing complex applications. A site page can be one of four types:

- **Action:** Displays a start form for a process model that allows users to initiate a process, like submitting a new service request
- **Interface:** Displays an interface object. Interface type pages are the only pages that can be added to page groups or use URL parameters in `a!urlForSite()`
- **Record List:** Displays a record list configured in a record type
- **Report:** Displays a report object

**Note:** For the purposes of this prompt, we will focus on creating a detailed description of all the **interfaces** that can be used in an Appian application.

#### **Appian Platform Constraints**
When designing screens, adhere to these Appian-specific constraints:

- **NEVER create a login screen** - Appian already has built-in authentication
- **NEVER create user management screens unless explicitly stated in the requirements** - Appian has native user/group administration
- **NEVER create role/permission management screens unless explicitly stated in the requirements** - Appian handles security groups natively
- **ALWAYS reference Appian record types** when displaying entity data
- **Use Appian's native document management** - don't design custom file storage interfaces

## INTERFACE TYPES

### Landing Page
Main entry point after login. Shows overview, recent activity, quick actions, and navigation to other screens.
**Example Use Cases:** Applicant Dashboard, Reviewer Dashboard, Admin Home

### Form
Data entry interface for creating or updating records.
**Types:**
- **Create/Edit Form** - Single interface used for both creating new records and updating existing records (controlled by `isUpdate` parameter)
- **Wizard** - Multi-step form with navigation between steps

**Design Principle:** To the extent possible, reuse the same interface for create, update, and delete operations. Use rule inputs to control behavior:
- `isUpdate` (boolean) - Indicates whether the interface is in create mode (false) or update mode (true)
- `isEditable` (boolean) - Controls whether all fields are editable or read-only
- `record` (Record Type) - The full record data structure being edited (null for create operations). Pass in the complete record with all field values rather than just an ID to avoid querying within the interface.

**Example Use Cases:** Application Form (create/edit), User Profile (view/edit), Course Registration Wizard

### Record List
Displays multiple records in grid/table format with search, filter, sort capabilities.
**Example Use Cases:** All Applications, Course Catalog, User Directory

### Record Instance
Detailed view of a single record with complete information and related data.
**Example Use Cases:** Application Details, Course Details, User Profile View

### Dashboard
Metrics, charts, and analytics displayed in tiles/cards for data visualization.
**Example Use Cases:** Review Metrics Dashboard, Application Statistics, Performance Overview

### Report
Formatted report optimized for printing or exporting to PDF/Excel.
**Example Use Cases:** Approved Applicants Report, Monthly Statistics, Audit Trail Report

## ANALYSIS METHODOLOGY

### Phase 1: Identify User Personas
1. Extract all user roles mentioned in requirements
2. Document each persona's responsibilities and access needs
3. Map personas to their primary workflows

### Phase 2: Map Workflows to Screens
For each workflow identified, determine:
- **Entry points** - Where users start their tasks
- **Data input needs** - What forms are required
- **Data viewing needs** - What lists, details, and reports are needed
- **Decision points** - What review/approval screens are required
- **Navigation flow** - How screens connect to each other

### Phase 3: Define Screen Specifications
For each screen, document:
- **What the user sees** - Layout, data fields, information displayed
- **What the user can do** - Available actions, buttons, operations
- **How they got there** - Navigation from other screens
- **Where they can go** - Navigation to other screens
- **What rules apply** - Validations, permissions, business rules

## OUTPUT FORMAT

For each screen, create a dedicated section with the following structure:

---

### Screen [Number]: [Screen Name]

#### Screen Type
[One of: Landing Page, Form, Record List, Record Instance, Dashboard, Report, Wizard]

#### Target Persona
[Primary user role(s) who will use this screen - use persona names from requirements]

#### Purpose and Business Context
[Why this screen exists, what problem it solves, what user need it addresses]

#### Interface Parameters (for Forms)
If this is a reusable form interface, document the rule inputs that control its behavior:
- **isUpdate** (boolean) - False for create mode, true for update mode
- **isEditable** (boolean) - Controls whether fields are editable or read-only
- **record** (CDT/Record Type) - The full record data structure being edited (null for create operations). Pass in the complete record with all field values rather than just an ID to avoid querying within the interface.
- [Any other parameters specific to this interface]

#### Data and Fields Displayed
[Detailed list of all information shown to the user, organized by section/group]

For each field, specify:
- Field name/label
- Data type (Text, Number, Date, Dropdown, etc.)
- Source (if data model available: reference entity.field_name; otherwise: describe conceptually)
- Required/optional status
- Special formatting or behavior

**Example with data model:**
- Customer Name: Text, from CUSTOMER.customer_name (required)

**Example without data model:**
- Customer Name: Text, from customer record (required)

**Organized by sections:**
- Section 1 Name:
  - Field 1: [description as above]
  - Field 2: [description as above]
- Section 2 Name:
  - ...

#### Available Actions and Buttons
[List all operations users can perform, including button labels and what they do]
- Action 1: [description, behavior, any conditions]
- Action 2: [description, behavior, any conditions]

#### Validation Rules
[Business rules that apply to this screen]
- Rule 1: [description]
- Rule 2: [description]

#### Navigation
- **How users access this screen:** [entry points, links from other screens]
- **Where users can go from here:** [exit points, links to other screens]

#### Related Data and Relationships
[What related entities, lists, or associated data are shown]

**If data model is available:**
- Reference specific entity names from the data model (e.g., "Related to APPLICATION entity via application_id foreign key")
- Include relationship types (one-to-many, many-to-many, etc.)

**If data model is NOT available:**
- Describe relationships conceptually (e.g., "Related to applications through applicant relationship")
- Note: "Data model reference needed for exact entity/field names"

#### Requirements Source
[Direct quote from requirements document with section reference]

#### Priority
[High / Medium / Low with brief justification]

---

**General Guidelines:**
- Use clear, descriptive names for screens
- For forms, always document interface parameters (isUpdate, isEditable, record, etc.)
- Group related fields into logical sections
- Be specific about data types, validation rules, and business logic
- Include exact button labels and action descriptions
- Trace every screen to specific requirements
- **If data model is available:** Reference exact entity and field names from the data model
- **If data model is NOT available:** Use descriptive entity references and note that exact names need to be confirmed

## EXAMPLES

### Example 1: Good Screen Description (Record Instance)

---

### Screen 1: Case Details View

#### Screen Type
Record Instance

#### Target Persona
Case Manager

#### Purpose and Business Context
Provides case managers with a comprehensive view of all case information in a single screen to efficiently manage and resolve customer issues. Enables managers to review case history, add comments, update status, and take actions without navigating away from the case.

#### Interface Parameters
N/A (read-only view with action buttons)

#### Data and Fields Displayed

**Case Information Section:**
- Case Number: Text, auto-generated unique identifier
- Title: Text, brief case summary
- Description: Long text, detailed issue description
- Category: Dropdown value (Technical, Billing, General Inquiry, Feature Request)
- Priority Level: Dropdown value with color coding (Low=green, Medium=yellow, High=orange, Critical=red)
- Current Status: Dropdown value (New, In Progress, Waiting on Customer, Resolved, Closed)

**Customer Information Section:**
- Customer Name: Text, linked to customer record
- Email: Text with mailto link
- Phone: Text with click-to-call functionality
- Company Name: Text
- Account Number: Text

**Assignment Details Section:**
- Assigned Agent Name: User picker, linked to user profile
- Assignment Date: Date/time stamp
- Due Date: Date with countdown or overdue indicator
- SLA Status: Visual indicator (green=on track, yellow=at risk, red=breached)

**Case History Grid:**
- Timestamp: Date/time of change
- User: Who made the change
- Previous Status: Status before change
- New Status: Status after change
- Comments: Notes added with the change

**Related Cases Table:**
- Related Case Number: Linked to case record
- Relationship Type: Dropdown (Duplicate, Related, Caused By)
- Status: Current status of related case
- Resolution: How related case was resolved

**Attachments List:**
- File Name: Clickable link to download
- Upload Date: Date/time uploaded
- Uploaded By: User who uploaded
- File Type: Icon indicating file type
- File Size: Size in KB/MB
- Download Link: Action to download file

**Comments/Notes Section:**
- Chronological list showing: timestamp, author name, comment text
- New comment entry field: Rich text area with formatting toolbar

**Resolution Information:**
- Resolution Type: Dropdown (Resolved - Customer Issue, Resolved - System Issue, Closed - Duplicate, etc.)
- Resolution Notes: Long text field
- Resolution Date: Date/time of resolution
- Customer Satisfaction Rating: 1-5 star rating

#### Available Actions and Buttons
- **Update Status:** Opens dropdown to change case status with required comment field
- **Reassign Case:** Opens user picker to assign to different agent
- **Add Comment:** Focuses on comment entry field
- **Attach File:** Opens file upload dialog (supports PDF, DOC, images, max 10MB)
- **Escalate:** Increases priority and notifies manager
- **Merge with Another Case:** Opens case search to select duplicate case
- **Close Case:** Closes case with required resolution type and notes

#### Validation Rules
- Status cannot be changed to Resolved or Closed without resolution information
- Reassignment requires selection of available agent
- Comments are required when changing status to certain values (Waiting on Customer, Resolved, Closed)
- File attachments must be under 10MB

#### Navigation
- **How users access this screen:** Clicking on case number from Case List, Dashboard, or search results; Deep link from email notifications
- **Where users can go from here:** Back to Case List, Customer Profile (via customer name link), Related Cases (via case number links), User Profile (via assigned agent link)

#### Related Data and Relationships
- Related to CUSTOMER entity (displays customer information)
- Related to USER entity (assigned agent, created by, modified by)
- Related to CASE_COMMENT entity (displays all comments)
- Related to ATTACHMENT entity (displays all attached files)
- Related to CASE entity via self-join (related cases)

#### Requirements Source
"Case managers need to view all case information in a single screen to efficiently manage and resolve customer issues. They should be able to update status, add comments, and take actions without navigating away" (Case Management Requirements section, page 12)

#### Priority
High - Core functionality for primary user persona, critical for daily operations

---

### Example 2: Good Screen Description (Reusable Form)

---

### Screen 2: Case Form

#### Screen Type
Form (Create/Edit)

#### Target Persona
Customer Service Representative, Case Manager

#### Purpose and Business Context
Single reusable interface for both creating new cases and editing existing cases. When used in create mode, guides representatives through capturing all necessary case information while the customer is on the phone. In edit mode, allows case managers to update case details.

#### Interface Parameters
- **isUpdate** (boolean) - False when creating new case, true when editing existing case
- **isEditable** (boolean) - Controls whether fields are editable or read-only (for view-only mode)
- **caseId** (integer) - The ID of the case being edited (null when creating new case)
- **customerId** (integer) - Pre-populate customer when launching from customer record (optional)

#### Data and Fields Displayed

**Customer Section:**
- Customer Search/Select: Type-ahead search of existing customers (only shown when isUpdate=false and customerId is null)
- Customer Name: Text (auto-populated from selected customer or editable for new customer)
- Email: Text with email validation (required)
- Phone: Text with phone format validation
- Company: Text (optional)

**Case Details Section:**
- Case Title: Text input, max 200 characters (required)
- Description: Rich text editor with formatting toolbar (required)
- Category: Dropdown (Technical, Billing, General Inquiry, Feature Request) - required
- Priority: Dropdown (Low, Medium, High, Critical) - required, defaults to Medium
- Status: Dropdown (only shown when isUpdate=true, disabled when isUpdate=false as it defaults to New)

**Assignment Section:**
- Assigned Agent: User picker with auto-suggest based on category and current workload (optional, can be left unassigned)
- Due Date: Date picker, auto-calculated based on priority and SLA rules, can be manually overridden

**Attachments Section:**
- File Upload: Multi-file upload component supporting PDF, DOC, DOCX, XLS, XLSX, PNG, JPG (max 10MB per file, max 5 files)
- Uploaded Files List: Shows file name, size, with remove button

#### Available Actions and Buttons
- **Save:** Validates all required fields and saves case
  - When isUpdate=false: Creates new case, sends confirmation email to customer, redirects to Case Details View
  - When isUpdate=true: Updates existing case, logs change in case history, stays on form with success message
- **Save as Draft:** (Only shown when isUpdate=false) Saves incomplete case as draft status
- **Cancel:** Returns to previous screen with confirmation dialog if unsaved changes exist
- **Delete:** (Only shown when isUpdate=true and user has delete permission) Soft-deletes case with confirmation

#### Validation Rules
- Email must be valid email format
- Phone number must match format (###) ###-####
- Case Title is required and cannot exceed 200 characters
- Description is required and must have at least 10 characters
- Category and Priority are required
- Duplicate case detection: Warns if similar case title exists for same customer within last 30 days
- File size cannot exceed 10MB per file
- Maximum 5 files can be attached

#### Navigation
- **How users access this screen:**
  - Create mode: "Create New Case" button from Dashboard, Case List, or Customer Profile
  - Edit mode: "Edit" button from Case Details View
- **Where users can go from here:**
  - After save in create mode: Redirects to newly created Case Details View
  - After save in edit mode: Stays on form or returns to Case Details View
  - Cancel: Returns to previous screen

#### Related Data and Relationships
- Creates/updates CASE entity
- Related to CUSTOMER entity (links case to customer)
- Related to USER entity (assigned agent)
- Related to ATTACHMENT entity (file attachments)

#### Requirements Source
"Representatives must be able to quickly create cases with all necessary information while the customer is on the phone. The system should guide them through required fields and prevent incomplete submissions" (Case Creation Process section, page 8)

"Case managers need ability to update case details as new information becomes available" (Case Management section, page 12)

#### Priority
High - Critical for case creation workflow and case management

---

### Example 3: Poor Screen Description (Don't Do This)

---

### Screen X: Case Screen

#### Screen Type
Form

#### Target Persona
User

#### Purpose and Business Context
Shows case info

#### Data and Fields Displayed
Case fields

#### Available Actions and Buttons
Save button

#### Requirements Source
N/A

#### Priority
Medium

**Why this is poor:**
- No specific screen name
- Vague persona ("User" instead of specific role)
- No detail about what information is displayed
- Missing interface parameters for reusable form
- No validation rules
- No navigation details
- No requirements traceability
- Generic priority without justification

## QUALITY CHECKLIST

Before finalizing screen specifications, verify:

☐ Every screen traces to specific requirement quote
☐ Every user persona has appropriate screens
☐ All CRUD operations are covered (Create, Read, Update, Delete where applicable)
☐ Navigation flow is complete (no dead ends or orphaned screens)
☐ Security considerations noted (who can access each screen)
☐ No duplicate or overlapping screens
☐ Screen descriptions are detailed enough for implementation
☐ Platform constraints observed (no login, user management, role administration)
☐ All interface types are properly categorized
☐ Priority levels are assigned based on user impact
☐ Business rules and validations are documented
☐ Related data and relationships are specified
