# Northwind Data Warehouse 
A learning project that turns the **Northwind** sample database into a layered data warehouse using **Microsoft SQL Server** and **T-SQL**. The repository documents the warehouse conventions and includes diagrams created in **draw.io** to explain the data architecture and data flow.

## Project goals

- Practice building a data warehouse from a relational source dataset.
- Understand how data moves from source tables through staging and warehouse layers toward analytical consumption.
- Apply repeatable naming standards to schemas, tables, columns, keys, procedures, constraints, indexes, and scripts.
- Document the warehouse through a data catalog and visual architecture/data-flow diagrams.

## Architecture at a glance

```text
Northwind source
      |
      v
   stage
      |
      v
    dds
      |
      v
sale / other subject-area data marts
      |
      v
BI reports and analytics
```

- **`stage`** — source-aligned data with minimal transformation, retaining traceability to Northwind.
- **`dds`** — the core warehouse layer where data is cleaned, standardized, integrated, and prepared for analysis.
- **Data marts** — subject-focused structures (for example, `sale`) designed for reporting and analytics.

The diagram above summarizes the documented logical flow. Refer to the draw.io files in the repository for the project-specific architecture and flow diagrams.

## Repository documentation

| Document | What it covers |
|---|---|
| `data_catalog.md` | Business definitions, layer responsibilities, table grain, data inventory, cleansing rules, ETL transaction handling, data quality checks, KPI definitions, security, and logging. |
| `naming_conventions.md` | Naming rules for schemas, tables, columns, keys, views, stored procedures, constraints, indexes, and project files. |
| Draw.io diagrams | Visual representation of the data architecture and the movement of data through the warehouse. |

> File names in this table describe the recommended repository names. If your repository currently uses different names, keep the links and names aligned with the actual files you commit.

## Technology

- Microsoft SQL Server
- T-SQL
- Northwind sample database
- draw.io for architecture and data-flow diagrams
- Markdown for technical documentation

## Suggested workflow

1. Prepare the Northwind source database in SQL Server.
2. Run the project's T-SQL scripts in the intended order, following any setup or dependency notes in the scripts.
3. Validate that the expected schemas and warehouse objects are created and populated.
4. Review the architecture and data-flow diagrams against the actual SQL implementation.
5. Use the data catalog and naming playbook as the reference when reviewing or extending the project.

## Naming principles

The project follows lowercase **`snake_case`** naming. The main patterns are:

- Staging tables: `<source_system>_<entity>`
- DDS dimensions, facts, and bridge tables: `dim_<entity>`, `fact_<entity>`, `bridge_<entity>`
- Surrogate keys: `<entity>_key`
- Source/business identifiers: `<entity>_id`
- Warehouse technical metadata: `dwh_<column_name>`
- Views: `vw_<entity_or_purpose>`
- Stored procedures: `usp_<action>_<layer_or_schema>_<entity>`

See the naming playbook for the complete rules and examples.

## Documentation alignment note

The two reference documents currently describe layers using different vocabularies: the catalog uses `stg` / `dim` / `fact`, while the naming playbook uses `stage` / `dds` / `sale`. The catalog also contains a generic sales-oriented inventory, whereas the naming playbook gives Northwind-specific examples. These concepts can coexist as documentation patterns, but the final repository should make clear which schema names and objects are actually implemented by your T-SQL scripts. Before submission, align the object inventory and architecture labels with the real database and diagrams rather than presenting examples as confirmed implementation details.

## Scope and learning context

This repository is an educational BI/data-warehouse exercise based on the Northwind dataset. The SQL implementation and documentation should be read together: the scripts show how objects are built, while the catalog and diagrams explain their intended meaning and relationships.

## Notes

- Run scripts only in a development or learning database where you have permission to create and modify objects.
- Do not commit passwords, connection strings containing secrets, access tokens, or other credentials.
- If a schema, table, column, transformation, or KPI changes, update the related documentation and diagrams so they remain consistent with the implementation.

---
**Project:** Northwind Data Warehouse  
**Purpose:** Business Intelligence / Data Warehouse course project
