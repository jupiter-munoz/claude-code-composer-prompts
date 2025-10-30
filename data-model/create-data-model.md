# Combined Data Model Extraction Prompt v2

You are extracting business entities and relationships from requirements documents to design a comprehensive data model for an OLTP system. Focus on accuracy over completeness - an incomplete but accurate model is better than one with fabricated entities. Additionally, provide structured enhancement suggestions that address evident gaps while maintaining strict source traceability.

## CORE PRINCIPLES

### **1. Source Traceability Rule**
- Every required entity must either trace to specific text in the source document OR be created due to database management best practices
- Quote exact source text for each entity you create from source requirements
- Mark unclear implications as "Requires validation" with specific explanation in Gaps section

### **2. Fabrication vs. Database Management vs. Enhancement**
**FORBIDDEN (Fabrication):** Adding entities not described in source requirements AND not needed for database management best practices AND not addressing evident process gaps

**REQUIRED (Source-Based):** Creating tables to store data explicitly described in business processes

**REQUIRED (Technical):** Database management best practices (primary keys, foreign keys, reference table structure) marked as "Best Practice"

**OPTIONAL (Suggested):** Process improvement enhancements that address evident gaps in audit trail, efficiency, or data integrity, marked as "Suggested"

### **3. Platform Assumptions**
- User/role tables: Use Appian's native user management (don't create custom user tables)
- Documents: Reference Appian document IDs, don't recreate document management

### **4. Enhancement Methodology**
- **REQUIRED (Source-Based):** Creating tables/fields to store data explicitly described in business processes
- **REQUIRED (Technical):** Database management best practices marked as "Best Practice"
- **OPTIONAL (Suggested):** Process improvement enhancements that address common data model gaps evident from the source requirements, marked as "Suggested"
- Enhancement suggestions must solve real problems evident in the source requirements without inventing new business processes

### **5. OLTP Database Design Constraints**
**Fourth Normal Form Requirement:**
- All tables must conform to Fourth Normal Form (4NF) to eliminate multi-valued dependencies
- **Exception:** Tables explicitly designed for reporting, analytics, or business intelligence may use denormalized structures for performance
- Mark any reporting/BI tables clearly as "Reporting Table" in the Business Purpose

**Appian Platform Constraints:**
- **NEVER use composite primary keys** - Appian cannot create record types from tables with composite keys
- **ALWAYS use surrogate keys** - Create single-column INTEGER primary keys for all tables
- **Junction Tables:** Must have surrogate primary key PLUS the two foreign keys that create the many-to-many relationship

## DATA TYPES AND CONSTRAINTS

### **Valid Data Types**
The only valid type values are: **TEXT, INTEGER, DECIMAL, DATE, DATETIME, BOOLEAN, USERNAME, GROUP, and DOCUMENT**

### **Data Type Mapping Guidelines**
- **TEXT**: For all string data (use max_length property to specify VARCHAR equivalent)
- **INTEGER**: For whole numbers and surrogate keys
- **DECIMAL**: For monetary values and precise calculations
- **DATE**: For date-only values
- **DATETIME**: For timestamps
- **BOOLEAN**: For yes/no values
- **USERNAME**: For references to Appian users (replaces custom user tables)
- **GROUP**: For references to Appian groups
- **DOCUMENT**: For file uploads and document references

### **Enhanced Attribute Properties**

**Data Integrity:**
- `primary_key: true` - Identifies primary key fields
- `auto_increment: true` - For auto-generated sequential IDs
- `unique: true` - Enforces uniqueness constraints
- `foreign_key: "TABLE.FIELD"` - Explicit foreign key relationships

**Validation & Constraints:**
- `max_length: number` - Character limits for TEXT fields
- `min_length: number` - Minimum character requirements
- `validation: "pattern_name"` - Built-in validation (email_format, phone_format, url_format, etc.)
- `allowed_values: [list]` - Enumerated value constraints for simple reference lists
- `min_value: number` - Minimum numeric values
- `max_value: number` - Maximum numeric values

**User Experience:**
- `default_value: "value"` - Default field values to reduce data entry
- `searchable: true` - Fields that should be indexed for search functionality
- `display_name: "Custom Label"` - User-friendly field labels
- `help_text: "guidance"` - Additional user guidance text
- `placeholder: "example"` - Input field placeholder examples

**Business Logic:**
- `calculated: true` - Fields computed from other values
- `read_only: true` - Fields that cannot be edited after creation
- `conditional_required: "condition"` - Fields required only under certain conditions

**Performance & Technical:**
- `indexed: true` - Fields requiring database indexes for performance
- `encrypted: true` - Sensitive data requiring encryption

**Source Traceability (Extensions):**
- `source: "Requirements|Best Practice|Suggested"` - Origin of this field
- `source_quote: "exact text"` - Direct quote from requirements (when source is Requirements)

## ENTITY DECISION FRAMEWORK

For each business concept, use this enhanced decision process:

**Step 1:** Is it a subject performing actions? → Primary Entity
**Step 2:** Is it a predefined list for selection? → Reference Table
**Step 3:** Does it connect entities many-to-many? → Junction Table
**Step 4:** Otherwise → Field in existing entity
**Step 5:** Does this enhancement address a gap in audit trail, process efficiency, or data integrity evident from the described business processes? → Suggested Enhancement

**Enhanced Decision Criteria:**
- **Create separate table if:** Removing this concept would require deleting business rules or workflows
- **Add fields if:** This concept only adds descriptive information to an existing entity
- **Suggest enhancement if:** This addresses a process gap evident in source requirements without changing core business logic

**Validation Questions for Each Potential Entity:**
- What business process creates this data?
- What business process modifies this data?
- What business rules govern this data?
- Does this data have independent lifecycle from other entities?

## ENHANCEMENT SUGGESTION GUIDELINES

**Audit and Compliance Enhancements:**
- Timeline tracking for regulatory requirements
- Detailed note/communication logging
- Calculation transparency for complex business rules
- Document quality and verification improvements

**Process Efficiency Enhancements:**
- Structured review checklists based on implied review processes
- Assignment and workload management capabilities
- Progress tracking and metrics collection
- Automated validation where business rules suggest patterns

**User Experience Enhancements:**
- Better status tracking and communication
- Workflow optimization based on described pain points
- Self-service capabilities that reduce manual overhead

**FORBIDDEN Enhancement Types:**
- New business processes not described in source
- Changes to regulatory requirements or forms
- Integration with systems not mentioned in source
- User roles or permissions beyond what's described

## ANALYSIS PROCESS

### **Phase 1: Comprehensive Document Analysis**

#### **Systematic Document Review Checklist:**
☐ **Main Document:** Business processes, entity descriptions, workflow rules
☐ **Embedded Forms:** Every input field, checkbox, dropdown, validation rule
☐ **Appendices:** Additional requirements, enumerated values, business rules
☐ **Workflow Diagrams:** Process states, decision points, data transformations
☐ **Business Rules Sections:** Validation criteria, temporal constraints, approval logic

#### **Form Field Mapping**
For each form element, consider business context:
- Input fields → Entity attributes or foreign keys
- Checkboxes (single) → Boolean fields
- Checkboxes (multiple) → Evaluate business context:
  - If selections represent **relationships between distinct entities** (e.g., assigning entities to entities) → Junction tables
  - If selections are **independent many-to-many relationships** (e.g., multiple categories for an item where categories are managed entities) → Junction tables
  - If selections are **attribute values from a fixed list** with no independent business rules → Use allowed_values
- Dropdowns → Reference tables if values need maintenance or have additional metadata; allowed_values if static and simple (2-10 values)
- File uploads → DOCUMENT fields
- Conditional fields → Business rule enforcement

#### **Evidence and Completion Pattern Recognition**
When source mentions "evidence of," "proof of," "completion of," "certification of," or "verification of":
- Create separate table if evidence has **business rules or workflows** (status tracking, expiration workflows, approval processes)
- Create separate table if evidence has **temporal validity** requiring business logic (expiration dates with renewal processes)
- Create separate table if evidence comes from **external sources requiring verification workflows**
- Use DOCUMENT field if evidence is simply **uploaded documentation** without additional business logic

#### **Temporal Business Rule Analysis**
Time-based constraints requiring separate tracking tables (not simple date fields):
- **Validity Windows with Business Rules:** "within X time period" + workflow/validation logic → Date tracking table
- **Expiration with Renewal Process:** "expires on" + renewal workflow → Status tracking table
- **Lifecycle Management:** "valid for X duration" + status transitions → Lifecycle management table
- **Simple Deadlines:** "must be completed before" → DATE field in existing entity (not separate table)

#### **Domain-Specific Patterns**
**Compliance/Regulatory Systems:** Look for evidence tracking, audit trails, approval workflows, watch lists

**Financial Systems:** Look for transaction patterns, approval hierarchies, reconciliation processes, document trails

**Educational Systems:** Look for completion tracking, prerequisite relationships, assessment data, progress monitoring

### **Phase 2: Entity Extraction and Design**

#### **Primary Entity Rules**
If source describes different business rules for two concepts, create separate entities:
- Different creation processes → Separate entities
- Different approval workflows → Separate entities
- Different data requirements → Separate entities
- Different user permissions → Separate entities

#### **Reference Table Design**
Create reference tables when values:
- Require maintenance or updates over time
- Need additional metadata (descriptions, display order, active status)
- Are used across multiple entities
- Represent process states or status codes with business rules
- Form classification systems or type hierarchies

Use `allowed_values` property instead of reference tables when values are:
- Static and unlikely to change (2-10 simple values)
- Used only in a single entity
- Simple strings with no additional metadata needed
- Truly fixed enumeration with no business logic

## COMMON ENTITY EXAMPLES

### **Evidence Tracking Patterns:**
- "Submit proof of insurance" → Insurance_Verification table (not just a DOCUMENT field)
- "Must complete safety training" → Training_Completion table (not just a boolean field)
- "Renew certification every 3 years" → Certification_Status table (not just an expiration date)
- "Employee holds multiple licenses" → Employee_License junction table

### **Temporal Validation Patterns:**
- "Valid for 2 years from completion" → Completion tracking table with date validation
- "Must renew before expiration" → Status tracking table with renewal workflow
- "Expires 30 days after issue" → Document lifecycle table

### **Multi-Selection Patterns:**
- "Select all applicable categories" (where categories are managed entities with their own data) → Category junction table
- "Assign multiple reviewers" (relationship between two entities) → Assignment junction table
- "Upload documents for each requirement" (documents are entities with metadata) → Requirement_Document junction table
- "Select applicable options" (from static fixed list with no entity relationships) → Use allowed_values property

## ANTI-PATTERNS TO AVOID

**Anti-Pattern 1:** Creating user tables when source mentions "reviewers," "approvers," or "administrators"
- **Correct:** Use USERNAME type for foreign key to Appian user management
- **Incorrect:** Create custom User, Role, or Permission tables

**Anti-Pattern 2:** Using single date fields for temporal validation
- **Correct:** Create tracking table when temporal validation involves business rules
- **Incorrect:** Add expiration_date field when source describes renewal processes

**Anti-Pattern 3:** Treating form sections as separate entities
- **Correct:** Analyze whether sections represent distinct business concepts
- **Incorrect:** Create table for every form section without business justification

**Anti-Pattern 4:** Using composite primary keys
- **Correct:** Create surrogate INTEGER primary key + unique constraint on business key combination
- **Incorrect:** PRIMARY KEY (field1, field2) - breaks Appian record type generation

**Anti-Pattern 5:** Violating 4NF in transactional tables
- **Correct:** Separate multi-valued dependencies into junction tables
- **Incorrect:** Storing multiple independent multi-valued attributes in same table

#### **Junction Table Requirements**
Create junction tables when source explicitly describes relationships between two primary entities with additional attributes:
- "Select multiple [items] from a list" where items and list represent different business entities
- "Assign [entities] to multiple [other entities]"
- "Configure [entity] with multiple [attributes]" where attributes have their own business logic
- Many-to-many relationships between distinct business entities
- Any scenario where the relationship itself has temporal attributes, status, or business rules

### **Phase 2B: Cross-Document Validation**

#### **Systematic Cross-Reference Process**
**Forms to Business Processes:**
- Verify every form field has corresponding data storage
- Confirm every "attachment required" has DOCUMENT field
- Validate every checkbox list has reference table or junction table

**Main Document to Appendices:**
- Cross-reference appendix content that clearly indicates data storage requirements
- Map appendix enumerated lists to reference tables when explicitly referenced in business processes
- Identify appendix business rules requiring data enforcement
- Flag appendix content that suggests data requirements but lacks clear business process context as "Requires validation"

**Conflict Resolution:**
- **Conflicting Requirements:** Document both versions and flag for stakeholder clarification
- **Missing Information:** Mark specific gaps rather than making assumptions
- **Unclear Scope:** Define entity boundaries based on clearest source evidence

### **Phase 3: Field Definition and Validation**

From source business processes, systematically identify:
- **Required vs. Optional:** Based on "must provide" vs "may provide" language
- **Validation Rules:** Explicitly stated constraints and business rules
- **Field Dependencies:** Conditional requirements and cascading validations
- **Calculated Fields:** Explicit calculation or derivation requirements
- **Business Rule Enforcement:** Where and how validation occurs in workflows

### **Phase 4: Enhancement Identification**
For each business process described in source, identify:
- **Audit Trail Gaps:** Where regulatory compliance suggests timeline tracking
- **Communication Inefficiencies:** Where "back and forth" or "follow-up" suggests structured note management
- **Process Visibility Gaps:** Where "status checking" or "progress tracking" suggests metrics collection
- **Manual Validation Overhead:** Where repetitive verification suggests structured checklists
- **Data Integrity Concerns:** Where complex calculations suggest transparency tables

### **Phase 5: Final Data Model Validation**

#### **Cross-Section Validation (Required)**
1. Verify every business process action has corresponding data storage
2. Confirm every reference table has values listed from source
3. Check every junction table explains the many-to-many relationship
4. Validate every DOCUMENT field traces to upload requirement
5. Cross-check all form fields against business process data storage
6. Verify all embedded document requirements are captured
7. Confirm all temporal business rules have appropriate tracking mechanisms
8. Validate all suggested enhancements address specific source problems

#### **Final Sanity Check**
- Can this data model support all mentioned business processes?
- Are there any orphaned entities with no clear business purpose?
- Do all relationships make logical business sense?
- Would removing any entity break described workflows?
- Do suggested enhancements solve real problems without inventing new processes?

## ERROR HANDLING GUIDANCE

### **Handling Ambiguous Situations**
**When Source is Unclear:**
- Document the ambiguity specifically
- Provide 2-3 possible interpretations
- Mark as "Requires validation" and include detailed explanation in Gaps section

**When Information is Missing:**
- Flag specific missing details rather than making assumptions
- Suggest prototyping scenarios to validate entity relationships
- Identify high/medium/low risk gaps with specific explanations

**When Requirements Conflict:**
- Document both conflicting requirements with source quotes
- Analyze which interpretation supports more business processes
- Recommend conflict resolution approach

### **Phase 5: Final Data Model Validation**

#### **Cross-Section Validation (Required)**
1. Verify every business process action has corresponding data storage
2. Confirm every reference table has values listed from source
3. Check every junction table explains the many-to-many relationship
4. Validate every DOCUMENT field traces to upload requirement
5. Cross-check all form fields against business process data storage
6. Verify all embedded document requirements are captured
7. Confirm all temporal business rules have appropriate tracking mechanisms
8. Validate all suggested enhancements address specific source problems

## NAMING STANDARDS

### **Table and Column Naming Rules**
- **Primary Keys:** [table_name]_id (INTEGER surrogate key - NEVER composite)
- **Foreign Keys:** [referenced_table]_id
- **Junction Tables:** [Entity1]_[Entity2] with surrogate primary key [entity1]_[entity2]_id
- **Status Fields:** [context]_status
- **Date Fields:** [context]_date
- **Boolean Fields:** is_[condition]
- **Do not use table prefixes**

### **Appian-Specific Column Naming Constraints**
- **Use snake_case consistently** for all database columns (Appian will convert to camelCase in record types)
- **Avoid SQL reserved keywords** as column names including but not limited to:
  - `ORDER`, `GROUP`, `USER`, `LEVEL`, `SIZE`, `TYPE`, `ROLE`
  - `DATE`, `TIME`, `TIMESTAMP`, `YEAR`, `MONTH`, `DAY`
  - `TABLE`, `VIEW`, `INDEX`, `KEY`, `VALUE`, `CONSTRAINT`
  - `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `WHERE`, `FROM`
  - `JOIN`, `UNION`, `COUNT`, `SUM`, `MAX`, `MIN`, `AVG`
- **Keep column names under 30 characters** for Oracle database compatibility
- If a reserved keyword must be used, prefix with context (e.g., `request_order` instead of `order`, `user_type` instead of `type`)

## DOCUMENT HANDLING GUIDELINES
- Use DOCUMENT data type for any file upload requirements
- For entities requiring multiple documents per record, create a separate table with a DOCUMENT field and a one-to-many relationship
- When creating separate tables for documents, use entity-specific naming like ENTITY_DOCUMENT (e.g., VEHICLE_DOCUMENT, MAINTENANCE_DOCUMENT) rather than a generic DOCUMENT table
- For entities requiring exactly one document per record, add a direct DOCUMENT field to that entity's table

## EXCLUDED FIELDS
Do not include the following audit fields in the base data model:
**CREATED_BY, CREATED_ON, MODIFIED_BY, MODIFIED_ON, and IS_ACTIVE**

These may be suggested as enhancements when business requirements explicitly need change tracking.

## OUTPUT FORMAT

**IMPORTANT:** Generate a YAML file as the primary output of this prompt.

**File Naming Convention:** `[ApplicationName]_Data_Model.yaml` (e.g., `USF_OSHA_Data_Model.yaml`)

**File Location:** Save in the `data-model/` folder of the project.

**Format:** Use the enhanced YAML structure below:

```yaml
# === DATA MODEL SUMMARY ===
summary:
  primary_tables: 0  # (0 Required + 0 Suggested)
  reference_tables: 0  # (0 Required + 0 Suggested)
  junction_tables: 0  # (0 Required + 0 Suggested)
  source_traceability: "100%"

# === PRIMARY ENTITIES ===
entities:
  - name: "ENTITY_NAME"
    description: "Business purpose description"
    
    # === ENHANCEMENTS (Optional - can be ignored by strict parsers) ===
    source_quote: "Exact text from source document"
    creation_trigger: "Business process that creates this entity"
    classification: "Required"  # Required|Suggested
    business_rules:
      - "Business rule 1 with source reference"
      - "Business rule 2 with source reference"
    problem_addressed: "For Suggested entities: specific gap solved"
    impact: "How this improves documented processes"
    priority: "High|Medium|Low"  # For Suggested entities
    
    # === CORE STRUCTURE (Required for compatibility) ===
    attributes:
      - name: "entity_id"
        type: "INTEGER"
        required: true
        primary_key: true
        auto_increment: true
        source: "Best Practice"
        description: "Unique identifier for Appian compatibility"
      - name: "business_field"
        type: "TEXT"
        required: true
        max_length: 200
        unique: false
        validation: "pattern_name"  # if applicable
        searchable: true
        indexed: true
        foreign_key: "REFERENCE_TABLE.reference_id"  # if applicable
        source: "Requirements"
        source_quote: "Specific text requiring this field"
        description: "Business purpose of this field"
    relationships:
      - type: "one-to-many"  # one-to-many|many-to-one|one-to-one
        entity: "RELATED_ENTITY_NAME"
        foreign_key: "related_entity_id"
        source_quote: "Text describing this relationship"
        business_rule: "When and why this relationship exists"
        description: "How these entities are related"

# === REFERENCE TABLES ===
reference_tables:
  - name: "REFERENCE_NAME"
    description: "Purpose of reference values"
    
    # === ENHANCEMENTS ===
    source_quote: "Where values are mentioned in requirements"
    classification: "Required"  # Required|Suggested
    problem_addressed: "For Suggested: gap this addresses"
    
    # === CORE STRUCTURE ===
    attributes:
      - name: "reference_id"
        type: "INTEGER"
        primary_key: true
        auto_increment: true
        source: "Best Practice"
        description: "Unique identifier"
      - name: "reference_code"
        type: "TEXT"
        required: true
        unique: true
        max_length: 20
        source: "Best Practice"
        description: "Short code for the reference value"
      - name: "reference_name"
        type: "TEXT"
        required: true
        max_length: 100
        source: "Best Practice"
        description: "Display name for the reference value"
      - name: "display_order"
        type: "INTEGER"
        required: false
        source: "Best Practice"
        description: "Order for displaying in lists"
    known_values:
      - "Value 1 from source"
      - "Value 2 from source"
      - "Value 3 from source"

# === JUNCTION TABLES ===
junction_tables:
  - name: "ENTITY1_ENTITY2"
    description: "Many-to-many relationship purpose"
    
    # === ENHANCEMENTS ===
    source_quote: "Many-to-many relationship description from source"
    classification: "Required"
    business_rule: "Rules governing this relationship"
    
    # === CORE STRUCTURE ===
    attributes:
      - name: "entity1_entity2_id"
        type: "INTEGER"
        primary_key: true
        auto_increment: true
        source: "Best Practice"
        description: "Surrogate key for Appian compatibility"
      - name: "entity1_id"
        type: "INTEGER"
        required: true
        foreign_key: "ENTITY1.entity1_id"
        indexed: true
        source: "Best Practice"
        description: "Foreign key to Entity1"
      - name: "entity2_id"
        type: "INTEGER"
        required: true
        foreign_key: "ENTITY2.entity2_id"
        indexed: true
        source: "Best Practice"
        description: "Foreign key to Entity2"
      - name: "relationship_date"
        type: "DATE"
        required: false
        source: "Requirements"
        source_quote: "If source mentions when relationship was established"
        description: "When this relationship was created"
    unique_constraints:
      - fields: ["entity1_id", "entity2_id"]
        description: "Prevent duplicate relationships"
```

## BUSINESS RULE DOCUMENTATION

For each entity, document how the data model supports:
- Validation rules mentioned in source (with specific quotes)
- Business process state management (status transitions)
- Business process enforcement (required workflows)
- Reporting and analytics requirements (specific reports mentioned)
- Temporal constraints and validation windows
- Evidence tracking and verification requirements

## ENHANCEMENT IMPACT ANALYSIS

For each suggested enhancement, document:
- **Process Improvement:** How it addresses source requirements pain points
- **Implementation Priority:** High/Medium/Low based on problem severity
- **Stakeholder Value:** Specific benefits to actors described in requirements
- **Compatibility:** How enhancement integrates with required entities

## GAPS AND VALIDATION REQUIREMENTS

**High-Risk Gaps:** [Issues that could break core business processes]
**Medium-Risk Gaps:** [Issues that could affect secondary features]
**Low-Risk Gaps:** [Minor clarifications needed]

**Stakeholder Validation Questions:**
- [Specific questions organized by business area]

**Enhancement Validation Questions:**
- [Questions about suggested improvement priorities and implementation approach]

**Prototype Scenarios:**
- [Suggested test scenarios to validate entity relationships]

## ITERATIVE REFINEMENT STEP

After completing initial analysis:

1. **Re-read source document** with the data model in mind
2. **Identify difficult scenarios:** Look for business processes that seem hard to support
3. **Verify relationship alignment:** Ensure all entity relationships align with described workflows
4. **Test with examples:** Walk through key business scenarios using the data model
5. **Validate enhancements:** Ensure suggested improvements address real source problems

## QUALITY CHECKLIST

**Critical Validations:**
☐ Every required table traceable to source quote
☐ Every "select multiple" has junction table or allowed_values
☐ Every "evidence of" or "completion of" pattern has appropriate tracking table
☐ Every temporal constraint has date tracking capability
☐ Every document upload has DOCUMENT field
☐ No platform infrastructure tables created (use USERNAME/GROUP types instead)
☐ All enumerated values have reference tables or allowed_values
☐ All form fields from embedded documents mapped to storage
☐ All suggested enhancements address specific source problems
☐ No suggested enhancements invent new business processes

**Platform Compatibility Validations:**
☐ All tables have single-column INTEGER primary keys
☐ No composite primary keys used anywhere
☐ All junction tables follow surrogate key + foreign key pattern
☐ All transactional tables conform to Fourth Normal Form
☐ Any denormalized tables clearly marked as reporting/BI tables
☐ All unique business key combinations implemented as unique constraints, not primary keys
☐ Only valid data types used (TEXT, INTEGER, DECIMAL, DATE, DATETIME, BOOLEAN, USERNAME, GROUP, DOCUMENT)

**Relationship Validations:**
☐ All foreign keys reference existing primary keys
☐ Junction tables connect exactly two entities
☐ Reference tables follow standard structure
☐ No orphaned entities without business purpose
☐ Suggested enhancements maintain compatibility with required entities

**Completeness Validations:**
☐ All business processes have data storage
☐ All form fields mapped to tables
☐ All embedded documents analyzed
☐ Cross-document validation completed
☐ All temporal business rules have tracking mechanisms
☐ All anti-patterns avoided
☐ Enhancement rationale clearly documented

**Output Completeness Validations:**
☐ YAML syntax is valid and parseable
☐ All entities have complete attribute definitions
☐ All foreign key relationships explicitly defined
☐ Enhanced properties applied where beneficial
☐ Source traceability maintained throughout
☐ Business rules documented with entities

## CRITICAL SUCCESS FACTORS

1. **Every required entity must trace to specific source text**
2. **Focus on evidence/completion patterns that need temporal tracking**
3. **Systematically analyze all forms and embedded documents**
4. **Use validation questions to test entity decisions**
5. **Don't create user/role tables (use USERNAME/GROUP types)**
6. **Document ambiguities rather than making assumptions**
7. **Always use surrogate INTEGER primary keys - never composite keys**
8. **Maintain Fourth Normal Form for all transactional tables**
9. **Apply systematic enhancement methodology while maintaining strict source traceability**
10. **Ensure YAML output is both human-readable and machine-parseable**

Remember: Your goal is creating a data model that supports all documented business processes while remaining strictly traceable to source requirements, enhanced with valuable suggestions that address evident gaps without fabricating new business functionality. The data model must be optimized for OLTP operations, fully compatible with Appian platform constraints, and delivered in a format that serves both implementation needs and documentation purposes.

## FINAL OUTPUT REQUIREMENT

**Primary Deliverable:** A complete, valid YAML file containing the full data model following the structure specified in the OUTPUT FORMAT section above.

**File Creation:** Use the Write tool to create the YAML file in the `data-model/` folder with naming convention `[ApplicationName]_Data_Model.yaml`.

**File Contents Must Include:**
- Data model summary with table counts
- All primary entities with complete attribute definitions
- All reference tables with known values
- All junction tables with relationships
- Business rule documentation
- Validation rules section
- Gaps and validation requirements section

The YAML file should be immediately usable by developers and parseable by automated tools while remaining human-readable for documentation purposes.