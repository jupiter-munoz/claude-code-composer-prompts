---
description: Generate a comprehensive data model for an Appian application from requirements document
---

Please analyze the provided requirements document and create a comprehensive data model following the methodology in data-model/create-data-model.md.

Requirements to analyze:
{{file}}

Generate:
1. Entity Summary with counts and traceability metrics
2. Complete table structures for all entities (Primary, Reference, Junction, and Suggested)
3. Entity relationships and foreign keys
4. Business rule implications
5. Enhancement impact analysis
6. Gaps and validation requirements

Ensure all tables use single-column INTEGER primary keys (Appian requirement) and conform to Fourth Normal Form for transactional tables.
