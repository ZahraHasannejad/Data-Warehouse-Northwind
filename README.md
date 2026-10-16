# 🏗️ Northwind Data Warehouse

**A layered Data Warehouse learning project built with Microsoft SQL Server and T-SQL.**

This project transforms the **Northwind sample database** into a layered data warehouse, demonstrating how data moves from a relational source system through staging and warehouse layers into subject-oriented data marts for business intelligence and analytics.

The repository also documents warehouse design conventions and includes **draw.io diagrams** illustrating the data architecture and data flow.

---

## 🎯 Project Goals

- 🗄️ Practice building a data warehouse from a relational source dataset.
- 🔄 Understand the data flow from source tables through staging and warehouse layers to analytical consumption.
- 📐 Apply consistent naming conventions for schemas, tables, columns, keys, procedures, constraints, indexes, and scripts.
- 📚 Document the warehouse using a data catalog and visual architecture and data-flow diagrams.

---

## 🏛️ Architecture Overview

The project follows a layered data warehouse architecture:

```text
┌─────────────────────────────┐
│       Northwind Source      │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│            stage            │
│   Source-aligned data       │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│             dds             │
│  Cleaned and integrated data│
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│        Data Marts           │
│     sale / other domains    │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│    BI & Data Analytics      │
└─────────────────────────────┘
```

### 🧱 Architecture Layers

| Layer | Responsibility |
|---|---|
| **`stage`** | Stores source-aligned data with minimal transformation while preserving traceability to Northwind. |
| **`dds`** | Cleans, standardizes, integrates, and prepares data for analytical use. |
| **Data Marts** | Organizes data into subject-focused structures, such as `sale`, for reporting and analytics. |
| **BI & Analytics** | Consumes curated data to support reporting and analytical use cases. |

📌 *The architecture above represents the documented logical flow. Refer to the draw.io files in the repository for the project-specific architecture and data-flow diagrams.*

---

## 📚 Repository Documentation

The repository includes documentation that explains the warehouse design, naming standards, and data movement.

| Document | Description |
|---|---|
| 📖 `data_catalog.md` | Business definitions, layer responsibilities, table grain, data inventory, cleansing rules, ETL transaction handling, data quality checks, KPI definitions, security, and logging. |
| 📝 `naming_conventions.md` | Naming standards for schemas, tables, columns, keys, views, stored procedures, constraints, indexes, and project files. |
| 🖼️ Draw.io Diagrams | Visual representations of the data architecture and the movement of data through the warehouse. |

> 💡 **Note:** The filenames above represent the recommended repository structure. If your existing files use different names, keep the README aligned with the actual files in your repository.

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| 🗄️ **Microsoft SQL Server** | Database platform |
| 💻 **T-SQL** | Data definition, transformation, and warehouse logic |
| 📦 **Northwind** | Sample relational source database |
| 🖼️ **draw.io** | Architecture and data-flow diagrams |
| 📝 **Markdown** | Technical documentation |

---

## 🚀 Suggested Workflow

Follow these steps to explore and validate the project:

1. **Prepare the source database** — Set up the Northwind sample database in SQL Server.
2. **Execute the SQL scripts** — Run the project's T-SQL scripts in the intended order, following setup instructions and dependencies.
3. **Validate warehouse objects** — Confirm that the expected schemas and database objects have been created and populated.
4. **Review the architecture** — Compare the architecture and data-flow diagrams with the actual SQL implementation.
5. **Consult the documentation** — Use the data catalog and naming conventions as references when reviewing or extending the project.

---

## 📏 Naming Conventions

The project follows **lowercase `snake_case` naming** to maintain consistency and readability across database objects and project files.

| Object Type | Naming Pattern | Example |
|---|---|---|
| Staging tables | `<source_system>_<entity>` | `northwind_orders` |
| DDS dimensions | `dim_<entity>` | `dim_customer` |
| DDS facts | `fact_<entity>` | `fact_sales` |
| DDS bridge tables | `bridge_<entity>` | `bridge_customer_order` |
| Surrogate keys | `<entity>_key` | `customer_key` |
| Source/business identifiers | `<entity>_id` | `customer_id` |
| Warehouse metadata columns | `dwh_<column_name>` | `dwh_load_date` |
| Views | `vw_<entity_or_purpose>` | `vw_customer_sales` |
| Stored procedures | `usp_<action>_<layer_or_schema>_<entity>` | `usp_load_stage_orders` |

📌 *These examples illustrate the naming patterns; they are not a guarantee that every listed object exists in the current implementation.*

For the complete rules and additional examples, refer to `naming_conventions.md`.

---

## 🔍 Documentation Alignment

The reference documents currently use different naming vocabularies:

- **Data catalog:** `stg` / `dim` / `fact`
- **Naming conventions:** `stage` / `dds` / `sale`

The catalog also contains a generic sales-oriented inventory, while the naming playbook includes Northwind-specific examples.

These concepts can coexist as documentation patterns, but the final repository should clearly distinguish **recommended conventions from implemented database objects**.

Before submitting the project:

- ✅ Align schema names and architecture labels with the actual SQL scripts.
- ✅ Verify that documented tables and columns match the implementation.
- ✅ Ensure the architecture and data-flow diagrams reflect the implemented data pipeline.
- ✅ Keep the data catalog, naming conventions, and SQL scripts consistent.

---

## 🎓 Project Scope

This repository is an **educational Business Intelligence and Data Warehouse project** based on the Northwind sample dataset.

The SQL scripts define how database objects are built and populated, while the documentation and diagrams explain their intended purpose, relationships, and place within the overall architecture.

The project provides a practical foundation for learning layered warehouse design, SQL-based data transformation, naming standards, and technical documentation.

---

## ⚠️ Important Notes

- 🧪 Run scripts only in a development or learning database where you have permission to create and modify objects.
- 🔐 Never commit passwords, access tokens, connection strings containing secrets, or other credentials.
- 🔄 When a schema, table, column, transformation, or KPI changes, update the related documentation and diagrams to keep everything consistent.

---

## 📌 Project Information

| Attribute | Details |
|---|---|
| **Project** | Northwind Data Warehouse |
| **Purpose** | Business Intelligence / Data Warehouse course project |
| **Database** | Northwind sample database |
| **Platform** | Microsoft SQL Server |
| **Language** | T-SQL |
| **Documentation** | Markdown |
| **Diagrams** | draw.io |

---

⭐ **Learning by building, documenting, and validating a real data warehouse workflow.**
