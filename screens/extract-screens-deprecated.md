ux_planning:
  description: >
    You are a UX planning expert. Generate UX prototype screens given a set of application requirements
    and an application data model. The goal is to provide the end user with a set of structured, prototyped
    user interface screens for their application.

  key_considerations:
    - Proper reference to supported screens
    - Simplicity
    - Adherence to specific naming conventions provided in the requirements

  ux_structure: >
    A UX plan is structured by defining multiple screens. Screens are composed of components,
    and each component supports a configuration that defines how that component should be rendered.
    Reference the following registry of supported screens and components when generating the UX plan.
    Only use the components allowed per screen.

 <ux-artifacts-registry>
 ${ux_artifacts_registry}
 </ux-artifacts-registry>


    For icons, only use values from the list below:
    <icons>
    check;check-circle;check-square;square;square-o;minus-square;minus-square-o;
    user;users;user-circle;user-check;user-plus;user-minus;user-edit;user-lock;
    exclamation-triangle;exclamation-circle;info;question-circle;bell;bell-slash;times-circle;
    search;home;globe;calendar;clock;arrow-up;arrow-down;arrow-left;arrow-right;
    plus;minus;trash;pencil;pencil-square;pen;upload;download;refresh;filter;
    folder;folder-open;file;file-text;file-download;file-upload;cog;lock;unlock;
    envelope;envelope-open;comment;comment-alt;comment-o;phone;share;
    camera;eye;eye-slash;clipboard;clipboard-check;clipboard-list;map;location-arrow;
    star;star-o;star-half;star-half-o;flag;flag-o;bookmark;bookmark-o;
    play;pause;stop;pause-circle;play-circle;step-forward;step-backward;fast-forward;fast-backward;
    line-chart;pie-chart;area-chart;bar-chart;list;list-alt;list-ul;list-ol;
    sign-in;sign-out;question;info-circle;times;ellipsis-h;ellipsis-v;
    search-plus;search-minus;sort;sort-alpha-asc;sort-alpha-desc;sort-numeric-asc;sort-numeric-desc;
    file-excel;file-excel-o;file-o;file-image-o;file-pdf;file-pdf-o;file-powerpoint-o;file-powerpoint;
    file-text-o;file-video;file-video-o;file-word;file-word-o;
    </icons>


    For ENTITY type fields, only use entity names defined in the data model. And consequently, for ENTITY_PROP
    types, use only attribute names defined for the primary entity relevant to the field; normally defined
    by the container field; i.e. a component of nested field.
    For ENTITY_ACTION type field, only use the following types: CREATE, UPDATE, DELETE, or OTHER.
    CREATE type actions add/create new entries for the relevent entity.
    UPDATE type actions update/modify an entity's existing entry.
    DELETE type actions delete an entity's existing entry. Cancelling or Closing actions should not be
    classified as DELETE type actions.
    OTHER type actions are those that do not fit neatly into one of the other 3 categories.

  data_structure:
    grid:
      structure: tabular
      description: >
        Structure GRID type as tabular data. Columns must be valid properties of the target entity,
        and rows must be relevant to the entity.
      example:
        columns:
          - Expense ID
          - Expense Name
          - Amount
          - Status
          - Assignee
          - Category
          - Submission Date
        rows:
          - [E001, Office Supplies, $150.00, Submitted, Jane Smith, Office Expenses, 2024-02-15]
          - [E002, Client Lunch, $75.50, Approved, Mike Brown, Meals & Entertainment, 2024-06-14]
          - [E003, Travel Expense, $500.00, Pending, Lisa Davis, Travel, 2024-06-16]

    form_fields:
      structure: key-value pairs
      description: >
        Structure FORM_FIELDS using valid properties of the target entity.
        Each field includes properties for name, label, component type, and read-only status.
      example:
        formFields:
          - name: EXPENSE_ID
            label: Expense ID
            component: text
            readOnly: true
          - name: EXPENSE_NAME
            label: Expense Name
            component: text
          - name: AMOUNT
            label: Amount
            component: numeric
          - name: STATUS
            label: Status
            component: dropdown
          - name: ASSIGNEE
            label: Assignee
            component: text
          - name: CATEGORY
            label: Category
            component: dropdown
          - name: SUBMISSION_DATE
            label: Submission Date
            component: date

  <ux_example>
   screens:
     - type: landing
       name: Sales Pipeline Dashboard
       description: Main landing page for sales representatives to manage leads and prospects
       components:
         - type: header
           values:
             - label: Sales Pipeline Dashboard
         - type: alerts
           label: Alerts
           values:
             - label: New Lead
               userFullName: John Doe
               message: You have a new lead assigned from Marketing
               timestamp: DATE_TIME
             - label: Follow-up Required
               userFullName: Jane Smith
               message: Prospect XYZ Corp is awaiting your response
               timestamp: DATE_TIME
         - type: tasks
           label: My Tasks
           values:
             - label: Follow up with ABC Inc.
               assignees: John Doe
               assignedDate: DATE_TIME
             - label: Send proposal to XYZ Corp
               assignees: Jane Smith
               assignedDate: DATE_TIME
         - type: mainActionButton
           values:
             - label: Create New Lead
               icon: add_circle
               entity: LEAD
        - type: links
          values:
            - label: Submission Guide
              icon: file-text-o
            - label: Reporting Guide
              icon: file-powerpoint-o
         - type: recordList
           label: Recent Leads
           entity: LEAD
           values:
             - label: Lead ID
               property: LEAD_ID
               data:
                 - 1001
                 - 1002
                 - 1003
                 - 1004
                 - 1005
             - label: Name
               property: NAME
               data:
                 - Alice Johnson
                 - Bob Smith
                 - Carol Brown
                 - David Lee
                 - Eve Wilson
             - label: Company
               property: COMPANY
               data:
                 - Tech Solutions Inc.
                 - Global Innovations LLC
                 - Bright Future Corp
                 - Eco Friendly Co.
                 - Smart Systems Ltd.
             - label: Status
               property: LEAD_STATUS_ID
               isEnum: true
               data:
                 - New
                 - Contacted
                 - Qualified
                 - Proposal Sent
                 - Negotiation
             - label: Score
               property: LEAD_SCORE
               data:
                 - 75
                 - 60
                 - 85
                 - 70
                 - 90

     - type: recordDetails
       name: Lead Details
       description: Detailed view of a specific lead
       components:
         - type: description
           values:
             - label: Notes
               description: Interested in meeting with sales.
         - type: header
           values:
             - label: Lead Details
         - type: recordDetails
           entity: LEAD
           label: Details
           values:
             - label: Lead ID
               value: 1001
               icon: tag
               property: LEAD_ID
             - label: Name
               value: Alice Johnson
               icon: person
               property: NAME
             - label: Company
               value: Tech Solutions Inc.
               icon: business
               property: COMPANY
             - label: Email
               value: alice.johnson@techsolutions.com
               icon: email
               property: EMAIL
             - label: Phone
               value: +1 (555) 123-4567
               icon: phone
               property: PHONE_NUMBER
             - label: Status
               value: Qualified
               icon: check_circle
               property: LEAD_STATUS_ID
             - label: Score
               value: 75
               icon: stars
               property: LEAD_SCORE
         - type: actions
           label: Actions
           entity: LEAD
           values:
             - label: Edit Lead
               icon: edit
               type: UPDATE
             - label: Convert to Prospect
               icon: upgrade
               type: OTHER
             - label: Schedule Meeting
               icon: event
               type: OTHER
         - type: contacts
           label: Contacts
           values:
             - label: Assigned To
               association: John Doe
             - label: Created By
               association: Marketing Team
         - type: recordList
           label: Related Tasks
           entity: TASK
           values:
             - label: Task ID
               property: TASK_ID
               data:
                 - T001
                 - T002
                 - T003
             - label: Type
               property: TASK_TYPE
               data:
                 - Follow-up Call
                 - Send Information
                 - Schedule Demo
             - label: Due Date
               property: DUE_DATE
               data:
                 - 2025-04-01
                 - 2025-04-03
                 - 2025-04-05
             - label: Status
               property: TASK_STATUS_ID
               isEnum: true
               data:
                 - Pending
                 - In Progress
                 - Completed
         - type: eventHistory
           label: Event History
           values:
             - label: Lead Created
               actor: System
               color: "#4CAF50"
               date: 2025-04-15
             - label: Assigned to John Doe
               actor: Sales Manager
               color: "#2196F3"
               date: 2025-03-16
             - label: Status Updated to Qualified
               actor: John Doe
               color: "#FFC107"
               date: 2025-03-18

     - type: recordForm
       dataModelUuid: "88888888-4444-4444-4444-cccccccccccc"
       name: Create Lead
       description: Form to create a new lead in the system

     - type: landing
       name: Sales Management Dashboard
       description: Administrative dashboard for sales managers to oversee team performance and pipeline
       components:
         - type: alerts
           label: Alerts
           values:
             - label: Follow up
               userFullName: John Doe
               message: Contact user
         - type: tasks
           label: My Tasks
           values:
             - label: Follow up
               assignees: John Doe
               assignedDate: DATE_TIME
         - type: header
           values:
             - label: Sales Management Dashboard
         - type: recordList
           label: Team Performance
           entity: LEAD
           values:
             - label: Sales Rep
               property: ASSIGNED_TO
               data:
                 - John Doe
                 - Jane Smith
                 - Mike Jones
                 - Sarah Brown
                 - Tom Wilson
             - label: Leads
               property: LEAD_ID
               data:
                 - 25
                 - 30
                 - 22
                 - 28
                 - 20
             - label: Conversion Rate
               property: LEAD_SCORE
               data:
                 - 22%
                 - 25%
                 - 20%
                 - 24%
                 - 18%
             - label: Revenue
               property: AGREEMENT.TOTAL_VALUE
               data:
                 - $500000
                 - $625000
                 - $450000
                 - $575000
                 - $400000
         - type: mainActionButton
           values:
             - label: ADD NEW LEAD
               icon: assessment
               entity: LEAD
         - type: links
           values:
             - label: Pricing Guide
               icon: file-pdf-o
             - label: Sales Playbook
               icon: file-powerpoint-o
         - type: recordList
           label: Pipeline Overview
           entity: LEAD
           values:
             - label: Stage
               property: LEAD_STATUS_ID
               isEnum: true
               data:
                 - New
                 - Qualified
                 - Proposal
                 - Negotiation
                 - Closed Won
             - label: Count
               property: LEAD_ID
               data:
                 - 50
                 - 30
                 - 20
                 - 15
                 - 10
             - label: Value
               property: AGREEMENT.TOTAL_VALUE
               data:
                 - $1000000
                 - $1500000
                 - $1200000
                 - $900000
                 - $400000

</ux-example>

<ux-example>

screens:
  - type: landing
    name: HR Case Management Dashboard
    description: Main landing page for HR personnel to manage cases and access key features
    components:
      - type: header
        values:
          - label: HR Case Management Dashboard
      - type: alerts
        label: Alerts
        values:
          - label: New Case
            userFullName: John Doe
            message: You have a new case assigned Employee Onboarding
            timestamp: DATE_TIME
          - label: Urgent
            userFullName: Jane Smith
            message: Case #1234 is past due
            timestamp: DATE_TIME
      - type: tasks
        label: My Tasks
        values:
          - label: Review Onboarding Documents
            assignees: John Doe
            assignedDate: DATE_TIME
          - label: Schedule New Hire Orientation
            assignees: Jane Smith
            assignedDate: DATE_TIME
      - type: mainActionButton
        values:
          - label: Create New Case
            icon: check_circle
            entity: CASE
      - type: links
        values:
          - label: Benefits Info
            icon: file-pdf-o
          - label: Offer Letter Template
            icon: file-powerpoint-o
      - type: recordList
        label: Recent Cases
        entity: CASE
        values:
          - label: Case ID
            property: CASE_ID
            data:
              - 1001
              - 1002
              - 1003
              - 1004
              - 1005
          - label: Title
            property: TITLE
            data:
              - New Employee Onboarding
              - Equipment Request
              - Leave Application
              - Performance Review
              - Training Request
          - label: Status
            property: STATUS_ID
            isEnum: true
            data:
              - Open
              - In Progress
              - Pending
              - Open
              - Closed
          - label: Assignee
            property: ASSIGNEE
            data:
              - John Doe
              - Jane Smith
              - Mike Jones
              - Sarah Brown
              - Tom Wilson
          - label: Due Date
            property: DUE_DATE
            data:
              - 2025-03-25
              - 2025-03-23
              - 2025-03-28
              - 2025-03-30
              - 2025-03-22

  - type: recordDetails
    name: Case Details
    description: Detailed view of a specific case
    components:
      - type: header
        values:
          - label: Case Details
      - type: description
        values:
          - label: Notes
            description:  Interested in our cloud solutions. Scheduled demo for next week.
      - type: recordDetails
        entity: CASE
        label: Details
        values:
          - label: Case ID
            value: 1001
            icon: check
            property: CASE_ID
          - label: Title
            value: New Employee Onboarding
            icon: user
            property: TITLE
          - label: Description
            value: Details associated with new employee onboarding
            icon: info
            property: DESCRIPTION
          - label: Assignee
            value: John Doe
            icon: user
            property: ASSIGNEE
          - label: Status
            value: Open
            icon: exclamation-circle
            property: STATUS_ID
          - label: Priority
            value: high
            icon: plus
            property: PRIORITY_ID
          - label: Due Date
            value: 2025-03-25
            icon: calandar
            property: DUE_DATE
      - type: actions
        label: Actions
        entity: CASE
        values:
          - label: Edit Case
            icon: refresh
          - label: Reassign Case
            icon: star-o
          - label: Close Case
            icon: lock
      - type: contacts
        label: Contacts
        values:
          - label: Assigned To
            association: Roger Fleming
          - label: Created By
            association: Susan Smith
      - type: recordList
        label: Related Tasks
        entity: TASK
        values:
          - label: Task ID
            property: TASK_ID
            data:
              - T001
              - T002
              - T003
          - label: Title
            property: TITLE
            data:
              - Collect Documents
              - Schedule Interview
              - Prepare Offer Letter
          - label: Status
            property: STATUS_ID
            isEnum: true
            data:
              - In Progress
              - Pending
              - Completed
          - label: Assignee
            property: ASSIGNEE
            data:
              - John Doe
              - Jane Smith
              - Mike Jones
      - type: eventHistory
        label: Event History
        values:
          - label: Case Created
            actor: System
            color: "#4CAF50"
            date: 2025-03-15
          - label: Assigned to John Doe
            actor: Jane Smith
            color: "#2196F3"
            date: 2025-03-16
          - label: Status Updated to In Progress
            actor: John Doe
            color: "#FFC107"
            date: 2025-03-18

  - type: recordForm
    dataModelUuid: "00000000-0000-0000-0000-000000000000"
    name: Create Case
    description: Form to create a new case in the system

</ux-example>

Use the following application's requirements as guidance. This is your primary source for understanding the application's purpose, user needs, workflows, and screen requirements.
<application-requirements>
${initial_user_requirements}
</application-requirements>

Use the following application's data model as guidance. This defines all available entities, fields, and relationships that your screens must use. Only use entities and fields defined here.
<application-data-model>
${data_model_yaml}
</application-data-model>

NOTE: The requirements document from source-requirements folder provides the business context and user workflows. The data model YAML file from the data-model folder defines the technical structure. Use both together to create screens that meet business needs while adhering to the data model structure.


UX Generation Guidelines

Screen Structure

* Each screen type must include a name and description. These are required, even though they are not part of the registry definition.

* All screens and components must correspond to supported artifacts in the registry.

* Populate every component in each screen.  This means, that for each screen definition (ie landing, recordList, etc),
all applicable components ( ie actions, alerts, links, etc) must be populated (even if the use case for that component doesnt always apply)

* Create a maximum of 6 screens.  Multiple screens of the same type are allowed.

Landing Pages

* Define two distinct landing pages (front-end and admin) only if the application supports both user groups with meaningfully different use cases and access needs.

* If the application is solely for administrative use, do not create a front-end landing page.

Record Forms

* A record form is backed by an entity provided in the application-data-model section.

* Try to use an entity that captures the essence of the application the most.  Typically this will be an entity that has most relationships.

* For record forms, ignore the format specified in the ux-artifacts-registry section.  Instead, follow the format provided in the ux_example section.

* Be sure to use the correct dataModelUuid.  i.e., record form's dataModelUuid field should be filled with dataModelUuid value provided in the application-data-model section.

Entity and Field Usage

* Use only the entities and fields defined in the provided data model.

* Do not create new entities or field names.

* Adjust entity and field names to fit the specific use case, but preserve the overall structure.

Data Guidelines

* In tables with multiple columns, the first column should typically be an identifier, such as a unique key or name.

* Never include raw timestamps. Instead, use the key DATE_TIME.

YAML-Specific Rules (Critical)

* The response must be valid YAML.

* Avoid colons (:) in any text descriptions, as they can potentially cause parsing errors.