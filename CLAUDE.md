# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

OSI (Open Semantic Interoperability) is a vendor-agnostic specification for standardizing semantic model exchange across data analytics, AI, and BI ecosystems. The project defines a YAML-based format for describing semantic models that can be shared between different tools and platforms.

## Repository Structure

- `core-spec/` - The core specification files
  - `spec.md` - Human-readable specification documentation (Version 1.0)
  - `spec.yaml` - YAML schema definition for the specification
  - `osi-schema.json` - JSON Schema (draft 2020-12) for YAML validation
- `validation/` - Validation tools
  - `validate.py` - Python validator for OSI semantic models
- `examples/` - Example semantic model files
  - `tpcds_semantic_model.yaml` - Complete TPC-DS retail benchmark example

## Schema Validation

The JSON Schema uses **Draft 2020-12** (latest stable specification) for modern `$defs` syntax and better `$ref` handling. Supported by Python jsonschema, VS Code YAML extension, and ajv.

Add this line to YAML files for VS Code validation:
```yaml
# yaml-language-server: $schema=../core-spec/osi-schema.json
```

Validate via CLI:
```bash
python validation/validate.py examples/tpcds_semantic_model.yaml
```

The validator checks:
- JSON Schema compliance
- Unique names (datasets, fields, metrics, relationships)
- Valid relationship references (from/to datasets must exist)
- SQL syntax validation via sqlglot (ANSI_SQL, SNOWFLAKE, DATABRICKS)

Install dependencies:
```bash
pip install pyyaml jsonschema sqlglot
```

## Specification Components

The OSI spec defines these main constructs (in hierarchical order):

1. **Semantic Model** - Top-level container with name, description, ai_context, datasets, relationships, metrics, and custom_extensions
2. **Datasets** - Logical entities (fact/dimension tables) with source, primary_key, unique_keys, and fields
3. **Relationships** - Foreign key connections between datasets using from/to with column arrays
4. **Fields** - Row-level attributes within datasets, supporting multi-dialect expressions
5. **Metrics** - Aggregate measures defined at semantic model level, can span multiple datasets

## Key Enumerations

**Dialects:** ANSI_SQL, SNOWFLAKE, MDX, TABLEAU, DATABRICKS

**Vendors (for custom_extensions):** COMMON, SNOWFLAKE, SALESFORCE, DBT, DATABRICKS

## Important Patterns

- Expressions support multiple SQL dialects for cross-platform compatibility
- `ai_context` fields throughout the spec provide context for AI tools (synonyms, instructions, examples)
- `custom_extensions` allow vendor-specific metadata without breaking core compatibility
- Primary keys and relationships support both simple and composite column arrays

## License

- Code: Apache 2.0
- Specification and documentation: Creative Commons Attribution (CC BY)
