---
name: database-connectivity
description: Helper for creating `tools.yaml` configuration files for the GenAI Toolbox. Use this skill when a user wants to configure a database connection (AlloyDB, Cloud SQL, Spanner) for the Toolbox or needs to generate a tools configuration file.
---

# Toolbox Config Generator

This skill assists users in creating a valid `tools.yaml` file for the GenAI Toolbox by collecting necessary database details and populating the appropriate template.

## Workflow

1.  **Identify Database Type**:
    Ask the user which database they want to configure:
    - Cloud SQL Postgres
    - Cloud SQL MySQL
    - AlloyDB Postgres
    - Spanner

2.  **Collect Information**:
    Based on the user's selection, request the **Required Information** listed in [references/templates.md](references/templates.md).
    *Note: Ask for all missing information in a single turn if possible, or guide the user step-by-step if they are unsure.*

3.  **Generate Configuration**:
    Once all information is collected:
    a.  Select the matching **Template** from [references/templates.md](references/templates.md).
    b.  Replace the placeholders (e.g., `<project_id>`, `<instance_name>`) with the user's provided values.
    c.  **Critical:** Ensure the output includes both the `sources` AND the `tools` sections exactly as shown in the template. Do not omit the `tools` section.
    d.  Present the complete `tools.yaml` content to the user in a code block.

4.  **Save Configuration**:
    Offer to save the generated configuration to the extension's directory.
    Target Path: `~/.gemini/extensions/mcp-db-context-enrichment/tools.yaml`

5.  **Validate Configuration**:
    After saving, run the validation script to ensure the configuration is valid:
    `node scripts/list_schemas.js`

## Templates & Reference

Refer to [references/templates.md](references/templates.md) for the specific fields required for each database type and the exact YAML structure to use.