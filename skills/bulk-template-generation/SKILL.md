---
name: bulk-template-generation
description: A guided workflow to automatically generate structured, natural language-to-SQL templates from a database schema. Use this when the user wants to "generate templates", "bulk create templates", or "enrich context" for a database.
---

# Bulk Template Generation

This skill guides you through the process of generating bulk Natural Language (NL) to SQL templates for a database.

## Workflow

Follow these steps sequentially to generate and save templates.

### 1. Discover and Select Database

1.  **Read Configuration**: Read the file `~/.gemini/extensions/mcp-db-context-enrichment/tools.yaml` to identify available database connections.
    -   If not present, use the database-connectivity skill to set it up.
2.  **Parse Sources**: Look for entries under the `sources` key. Each entry represents a connection.
    -   Identify `Connection` (the key, e.g., `alloydb-db`), `Instance` (`instance` field), and `DB` (`database` field).
3.  **User Selection**:
    -   If multiple databases are found, list them in the format: `Connection: <name> | Instance: <instance> | DB: <db>` and ask the user to choose one.
    -   If only one is found, present it and ask for confirmation.
    -   **Stop** and wait for user confirmation before proceeding.

### 2. Fetch Schema

1.  **Execute Script**: Run the schema fetching script for the selected database.
    ```bash
    node skills/bulk-template-generation/scripts/fetch_schema.js
    ```
2.  **Analyze Output**:
    -   Present a summary of the found tables to the user.

### 3. Scope Definition

Ask the user to specify which tables they want to generate templates for (or if they want to include "all tables").

### 4. Generate Question/SQL Pairs

Using the schema and the user's scope:

1.  **Generate Pairs**: Create a list of Question/SQL pairs.
    -   **Goal**: Coverage of common analytical questions for the selected tables.
    -   **Format**: JSON-like structure (internal).
2.  **Review Loop**:
    -   Present the generated pairs to the user for review.
    -   Format each pair clearly:
        ```text
        **Pair [N]**
        **Question:** [Natural Language Question]
        **SQL:**
        ```sql
        [SQL Query]
        ```
    -   Ask for approval or feedback.
    -   **Refinement**: If the user provides feedback, modify the pairs (edit specific ones or regenerate the list) and present again.
    -   Repeat until the user approves.

### 5. Generate ContextSet

Once the list is approved, convert the pairs into the final `ContextSet` JSON format.

**ContextSet Structure:**
```json
{
  "templates": [
    {
      "nl_query": "Question text",
      "sql": "SELECT ...",
      "intent": "Question text",
      "manifest": "Question text",
      "parameterized": {
        "parameterized_sql": "SELECT ...",
        "parameterized_intent": "Question text"
      }
    }
  ]
}
```
*Note: Ensure `sql_dialect` logic is applied if necessary (default to PostgreSQL).*

### 6. Save Templates

1.  **Choose Save Method**: Ask the user if they want to:
    -   Create a new file.
    -   Append to an existing file.

2.  **Perform Save**:
    -   **New File**: Generate a filename: `[instance]_[db]_context_set_[timestamp].json`. Write the JSON content to this file in the current working directory (or a `context_sets/` subdirectory if it exists).
    -   **Append**: Ask for the file path. Read the existing file, parse it, append the new `templates` to the `templates` array, and overwrite the file with the updated JSON.

3.  **Confirmation**: Confirm the file path to the user.