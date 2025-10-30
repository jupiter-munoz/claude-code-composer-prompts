---
description: Generate a comprehensive data model for an Appian application from requirements document
---

Please analyze the provided requirements document and create a comprehensive data model following the methodology in data-model/create-data-model.md.

Requirements to analyze:
{{file}}

**OUTPUT REQUIREMENT:** Generate a single YAML file as the sole output. Do not generate any other formats (markdown, tables, etc.).

The YAML file must include:
1. Entity Summary with counts and traceability metrics
2. Complete table structures for all entities (Primary, Reference, Junction, and Suggested)
3. Entity relationships and foreign keys
4. Business rule implications
5. Enhancement impact analysis
6. Gaps and validation requirements

**YAML Validation Rules:**
- All strings must be properly quoted if they contain special characters
- For multiple source quotes, use YAML list syntax: `source_quote: ["quote1", "quote2"]`
- Never concatenate strings with "and" outside of quotes
- Validate YAML syntax before outputting

Ensure all tables use single-column INTEGER primary keys (Appian requirement) and conform to Fourth Normal Form for transactional tables.
