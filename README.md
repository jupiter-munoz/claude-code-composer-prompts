# Composer Prompts for Appian Applications

A collection of specialized prompts for analyzing requirements and generating design artifacts for Appian applications.

## Overview

This project provides structured prompts and methodologies for:
- Extracting and structuring requirements
- Creating user personas
- Generating data models
- Designing workflows
- Specifying interfaces

## Project Structure

```
claude-code-composer-prompts/
├── .claude/
│   └── commands/                    # Claude Code slash commands
│       ├── data-model.md            # Generate data model
│       ├── requirements.md          # Extract requirements
│       └── personas.md              # Create personas
├── source-requirements/             # Source of truth requirements documents
│   ├── USF_OSHA_Prereqs_Solution_10_28_2025.docx
│   └── USF_OSHA_Prereqs_Solution_10_28_2025.txt
├── data-model/                      # Data model generation prompts
│   ├── create-data-model.md         # Data model extraction methodology
│   ├── USF_OSHA_Data_Model.md       # Example: Generated data model
│   └── USF_OSHA_Data_Model.yaml     # Example: YAML output
├── requirements_and_personas/       # Requirements and persona extraction
│   └── extract-requirements-and-personas.md
├── screens/                         # Screen and interface generation prompts
├── workflows/                       # Workflow design prompts
└── interfaces/                      # Interface design prompts
```

## Usage

### Using Claude Code Slash Commands

The easiest way to use these prompts is through Claude Code slash commands:

1. **Generate Data Model:**
   ```
   /data-model
   ```
   Then provide your requirements document when prompted.

2. **Extract Requirements:**
   ```
   /requirements
   ```
   Then provide your source document when prompted.

3. **Create User Personas:**
   ```
   /personas
   ```
   Then provide your requirements document when prompted.

4. **Generate Screen Specifications:**
   ```
   /screens
   ```
   Then provide your requirements document when prompted.

### Using Methodology Documents Directly

You can also reference the methodology documents directly in your prompts:

```
Using the methodology in data-model/create-data-model.md, please analyze
this requirements document and generate a comprehensive data model.

[paste your requirements]
```

### Typical Workflow

1. **Start with Source Requirements:** Place your requirements documents in `source-requirements/`
2. **Extract Requirements and Personas:** Use the requirements_and_personas prompt to generate structured XML
3. **Generate Data Model:** Use the data-model prompt to create YAML data model
4. **Design Screens:** Use the screens prompt to generate interface specifications
5. **Design Workflows:** Use workflow prompts to create process models (coming soon)

## Prompt Categories

### Data Model Generation
- **Location:** [data-model/create-data-model.md](data-model/create-data-model.md)
- **Purpose:** Generate comprehensive OLTP data models from requirements
- **Key Features:**
  - Fourth Normal Form compliance
  - Appian platform constraints (surrogate keys, no composite keys)
  - SQL reserved keyword avoidance
  - snake_case naming conventions
  - Source traceability for all entities
  - Enhancement suggestions
  - Junction table patterns
  - Reference table design
  - YAML output format

### Requirements and Persona Extraction
- **Location:** [requirements_and_personas/extract-requirements-and-personas.md](requirements_and_personas/extract-requirements-and-personas.md)
- **Purpose:** Extract structured requirements and user personas from documents
- **Key Features:**
  - Activity-based requirement structure (Features → Epics → Stories)
  - Persona identification with roles and responsibilities
  - Action type classification (User Action, Automated Process, External Integration, AI Process, etc.)
  - XML output format
  - Comprehensive workflow mapping

### Screen Design
- **Location:** [screens/extract-screens.md](screens/extract-screens.md)
- **Purpose:** Generate Appian interface and site page specifications from requirements
- **Key Features:**
  - Screen inventory from requirements
  - Component-level specifications
  - Field layouts and validations
  - Navigation flows
  - Persona-specific views
  - SAIL interface patterns
  - Responsive design considerations
  - Action and button specifications

## Coming Soon

### Workflow Design
Prompts for designing Appian process models, including:
- Process flow diagrams
- Decision logic
- Integration points
- Exception handling

### Interface Design
Prompts for designing Appian interfaces, including:
- Form layouts
- User experience patterns
- Navigation structure
- Dashboard design

## Best Practices

1. **Source Requirements First:** Store all original requirements in `source-requirements/` as the single source of truth
2. **Extract Requirements and Personas:** Begin by extracting structured requirements and identifying user personas
3. **Data Model Foundation:** Design your data model before workflows and interfaces
4. **Iterative Refinement:** Use validation questions to refine outputs
5. **Source Traceability:** Always maintain links to source documents in generated artifacts
6. **Follow Naming Conventions:** Use snake_case for database columns, avoid SQL reserved keywords

## Appian-Specific Considerations

These prompts are optimized for Appian platform constraints:

- **Data Models:** Use single-column INTEGER primary keys (no composite keys)
- **Normalization:** Fourth Normal Form for transactional tables
- **User Management:** Reference Appian's native user/group system
- **Document Management:** Reference Appian document IDs
- **Record Types:** Data model designed for Appian record type generation

## Contributing

To add new prompts:

1. Create a new folder for the prompt category
2. Add the methodology document (e.g., `create-[category].md`)
3. Create a slash command in `.claude/commands/[category].md`
4. Update this README with the new category

## Version History

- **v1.0** - Initial release with data model, requirements, and personas prompts
- Data Model Prompt: v6
- Requirements Prompt: v1
- Personas Prompt: v1

## License

Internal use for Appian application development.
