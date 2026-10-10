# Naming Playbook — Northwind Data Warehouse

This playbook sets a consistent naming standard for database objects across the Northwind warehouse layers and its subject-oriented data marts.

A shared naming standard makes database objects:

- consistent
- readable
- maintainable
- scalable
- business-aligned

> **Quick test:** A name should tell a teammate what the object represents, which layer it belongs to, and whether it is business data or warehouse metadata—without needing to open the SQL definition.

---

## Table of Contents

1. [The Naming Mindset](#1-the-naming-mindset)
2. [Layers at a Glance](#2-layers-at-a-glance)
3. [Schema Names](#3-schema-names)
4. [Table Names](#4-table-names)
   - [Staging Layer Rules](#41-staging-layer-rules)
   - [DDS Layer Rules](#42-dds-layer-rules)
   - [Data Mart Rules](#43-data-mart-rules)
5. [Column Names](#5-column-names)
   - [Business Attributes](#51-business-attributes)
   - [Surrogate Keys](#52-surrogate-keys)
   - [Business Keys](#53-business-keys)
   - [Foreign Keys](#54-foreign-keys)
   - [Technical Metadata Columns](#55-technical-metadata-columns)
6. [View Names](#6-view-names)
7. [Stored Procedure Names](#7-stored-procedure-names)
8. [Constraint Names](#8-constraint-names)
9. [Index Names](#9-index-names)
10. [Script and Documentation File Names](#10-script-and-documentation-file-names)
11. [Northwind Naming Gallery](#11-northwind-naming-gallery)
12. [Good Patterns and Naming Traps](#12-good-patterns-and-naming-traps)

---

## 1. The Naming Mindset

- **Naming style**: Use `snake_case` only.
- **Letter case**: Use lowercase letters only.
- **Language**: Use English for all database object names.
- **Separator**: Use underscore (`_`) to separate words.
- **Clarity**: Names must be meaningful and easy to understand.
- **Consistency**: Apply the same naming pattern across all schemas and layers.
- **Reserved words**: Do not use SQL reserved keywords as object names.
- **Abbreviations**: Avoid unclear abbreviations unless they are widely accepted and documented.
- **Business alignment**: Use business-friendly names in `dds` and data marts.
- **Plurality**: Use plural nouns for tables where appropriate, such as `dim_customers` and `fact_orders`.

---

## 2. Layers at a Glance

The database architecture consists of two main layers and one or more subject-oriented data marts.

| Layer / Schema Type | Name | Purpose |
|---|---|---|
| Staging layer | `stage` | Raw source-aligned data loaded from source systems |
| Core warehouse layer | `dds` | Cleansed, integrated, and historized warehouse data |
| Data mart | `sale` | Sales-focused reporting and analytics structures |

### How Data Moves Through the Layers

```text
Source System (Northwind)
        ↓
stage
        ↓
dds
        ↓
sale / other data mart schemas
```

### Future Data Mart Schema Examples

- `sale`
- `finance`
- `inventory`
- `crm`
- `hr`

---

## 3. Schema Names

Schema names must represent their technical layer or business domain.

### Rules to Apply

- Use lowercase English names.
- Keep schema names short, meaningful, and domain-oriented.
- Use `stage` exclusively for the staging layer.
- Use `dds` exclusively for the core data warehouse layer.
- Create separate schemas for each data mart business domain.

### Examples

| Schema | Type | Description |
|---|---|---|
| `stage` | Technical layer | Raw data ingestion and source traceability |
| `dds` | Technical layer | Core integrated data warehouse structures |
| `sale` | Data mart | Sales reporting and analysis |
| `finance` | Data mart | Financial reporting and analysis |
| `inventory` | Data mart | Inventory reporting and analysis |

---

## 4. Table Names

### 4.1 Staging Layer Rules

The `stage` layer stores raw data extracted from source systems. Transformations in this layer must be minimal and limited to technical loading requirements.

### Pattern

```text
<source_system>_<entity>
```

### Rules to Apply

- Table names must start with the source system name.
- The entity name should remain as close as possible to the original source table name.
- Do not apply business-oriented renaming in the `stage` layer.
- Use the schema name `stage` to identify the layer; use the table name to preserve source traceability.

### 11. Northwind Naming Gallery

- `stage.northwind_customers`
- `stage.northwind_orders`
- `stage.northwind_order_details`
- `stage.northwind_products`
- `stage.northwind_employees`
- `stage.northwind_suppliers`
- `stage.northwind_shippers`
- `stage.northwind_categories`

---

### 4.2 DDS Layer Rules

The `dds` layer contains the core Data Warehouse structures. Data is cleaned, standardized, integrated, and prepared for analytical consumption in this layer.

### Pattern

```text
<category>_<entity>
```

### Category Prefixes

| Prefix | Meaning | Example |
|---|---|---|
| `dim_` | Dimension table | `dim_customers` |
| `fact_` | Fact table | `fact_sales` |
| `bridge_` | Bridge table for many-to-many relationships | `bridge_employee_territories` |

### Rules to Apply

- Use business-aligned and source-independent names where possible.
- Dimension tables must use the `dim_` prefix.
- Fact tables must use the `fact_` prefix.
- Bridge tables must use the `bridge_` prefix.
- Table names must clearly describe their analytical purpose.

### Northwind DDS Examples

#### Dimensions

- `dds.dim_customers`
- `dds.dim_products`
- `dds.dim_employees`
- `dds.dim_suppliers`
- `dds.dim_categories`
- `dds.dim_shippers`
- `dds.dim_dates`
- `dds.dim_regions`
- `dds.dim_territories`

#### Facts

- `dds.fact_orders`
- `dds.fact_order_details`
- `dds.fact_sales`

#### Bridge Tables

- `dds.bridge_employee_territories`

---

### 4.3 Data Mart Rules

Data marts must be implemented as separate schemas based on business domains. For example, the sales data mart uses the `sale` schema.

### Pattern

```text
<mart_schema>.<category>_<entity>
```

### Rules to Apply

- The schema must identify the business domain.
- The table name must identify the analytical object.
- Use business-friendly names suitable for reporting tools and end users.
- Do not repeat the business domain in the table name when it is already represented by the schema.

### Recommended Naming

```text
sale.fact_sales
```

### Avoid Redundant Naming

```text
sale.fact_sale_sales
```

### Northwind Sales Data Mart Examples

#### Dimensions

- `sale.dim_customers`
- `sale.dim_products`
- `sale.dim_employees`
- `sale.dim_dates`
- `sale.dim_shippers`

#### Facts

- `sale.fact_sales`
- `sale.fact_order_details`

---

## 5. Column Names

### 5.1 Business Attributes

Business columns must use clear and descriptive names that represent their actual business meaning.

### Examples

- `customer_name`
- `company_name`
- `contact_name`
- `product_name`
- `category_name`
- `order_date`
- `required_date`
- `shipped_date`
- `unit_price`
- `quantity`
- `discount`
- `sales_amount`
- `ship_city`
- `ship_country`

### Avoid

- `name1`
- `value`
- `data_field`
- `temp_col`
- `col_a`

---

### 5.2 Surrogate Keys

Surrogate keys are warehouse-generated identifiers. Dimension tables must use surrogate keys as their primary keys.

### Pattern

```text
<entity>_key
```

### Rules to Apply

- Use the `_key` suffix exclusively for surrogate keys.
- The name must refer to the entity represented by the key.
- In SQL Server, use `int` or `bigint` according to expected data volume.
- Surrogate keys should not carry business meaning.

### Examples

- `customer_key`
- `product_key`
- `employee_key`
- `supplier_key`
- `category_key`
- `shipper_key`
- `date_key`
- `order_key`

### Example: `dds.dim_customers`

| Column | Description |
|---|---|
| `customer_key` | Warehouse-generated surrogate primary key |
| `customer_id` | Northwind business key |
| `company_name` | Customer company name |
| `contact_name` | Customer contact name |
| `country` | Customer country |

---

### 5.3 Business Keys

Business keys are identifiers that originate from the source system and have business or operational meaning.

### Pattern

```text
<entity>_id
```

### Rules to Apply

- Use the `_id` suffix for source-system identifiers.
- Preserve business keys in `dds` for traceability.
- Do not confuse business keys with surrogate keys.
- A business key may not be unique across multiple source systems; use source metadata where necessary.

### 11. Northwind Naming Gallery

- `customer_id`
- `product_id`
- `employee_id`
- `supplier_id`
- `order_id`
- `shipper_id`
- `category_id`
---

### 5.4 Foreign Keys

Foreign-key columns must use the same name as the referenced surrogate key.

### Pattern

```text
<referenced_entity>_key
```

### Example: `dds.fact_sales`

- `order_key`
- `customer_key`
- `product_key`
- `employee_key`
- `shipper_key`
- `date_key`

---

### 5.5 Technical Metadata Columns

Technical and audit columns must use the `dwh_` prefix.

### Pattern

```text
dwh_<column_name>
```

### Rules to Apply

- Use `dwh_` only for Data Warehouse technical metadata.
- Do not use `dwh_` for business attributes.
- Apply technical columns consistently to tables where they are required.

### Standard Technical Columns

| Column | Description |
|---|---|
| `dwh_load_date` | Date on which the record was loaded |
| `dwh_load_datetime` | Date and time on which the record was loaded |
| `dwh_batch_id` | Identifier for the ETL/ELT execution batch |
| `dwh_source_system` | Name of the originating source system |
| `dwh_record_source` | Source table, file, or endpoint identifier |
| `dwh_row_hash` | Hash used for change detection |
| `dwh_inserted_at` | Date and time at which the row was inserted |
| `dwh_updated_at` | Date and time at which the row was last updated |
| `dwh_is_deleted` | Logical deletion indicator |
| `dwh_valid_from` | Start date/time of the record validity period |
| `dwh_valid_to` | End date/time of the record validity period |
| `dwh_is_current` | Indicates the current active version of an SCD record |

### Example: `dds.dim_products`

- `product_key`
- `product_id`
- `product_name`
- `supplier_key`
- `category_key`
- `unit_price`
- `dwh_source_system`
- `dwh_batch_id`
- `dwh_load_datetime`
- `dwh_row_hash`

---

## 6. View Names

Views must use the `vw_` prefix.

### Pattern

```text
vw_<entity_or_purpose>
```

### Rules to Apply

- Place views in the schema where they are consumed.
- Use business-oriented names for reporting views.
- Do not use `view_` as a prefix.

### Examples

- `dds.vw_customer_orders`
- `dds.vw_sales_summary`
- `sale.vw_monthly_sales`
- `sale.vw_top_customers`
- `sale.vw_sales_by_country`
- `sale.vw_product_performance`

---

## 7. Stored Procedure Names

Stored procedure names must clearly describe their action, target layer or schema, and target entity where applicable.

### Pattern

```text
usp_<action>_<layer_or_schema>_<entity>
```

### Common Actions

- `load`
- `merge`
- `transform`
- `refresh`
- `rebuild`
- `validate`
- `archive`

### Stage Examples

- `usp_load_stage_northwind_customers`
- `usp_load_stage_northwind_orders`
- `usp_load_stage_northwind_order_details`
- `usp_load_stage_full`

### DDS Examples

- `usp_load_dds_dim_customers`
- `usp_load_dds_dim_products`
- `usp_load_dds_fact_orders`
- `usp_load_dds_fact_sales`
- `usp_load_dds_full`

### Sales Data Mart Examples

- `usp_refresh_sale_fact_sales`
- `usp_refresh_sale_full`

---

## 8. Constraint Names

Constraints must use a prefix that identifies their type.

| Prefix | Constraint Type | Pattern | Example |
|---|---|---|---|
| `pk_` | Primary key | `pk_<table>` | `pk_dim_customers` |
| `fk_` | Foreign key | `fk_<child_table>_<parent_table>` | `fk_fact_sales_dim_customers` |
| `uq_` | Unique constraint | `uq_<table>_<column>` | `uq_dim_customers_customer_id` |
| `ck_` | Check constraint | `ck_<table>_<rule>` | `ck_fact_sales_quantity_positive` |
| `df_` | Default constraint | `df_<table>_<column>` | `df_dim_dates_dwh_load_datetime` |

### Examples

```sql
constraint pk_dim_customers primary key (customer_key)
constraint fk_fact_sales_dim_customers
    foreign key (customer_key)
    references dds.dim_customers (customer_key)
constraint ck_fact_sales_quantity_positive check (quantity > 0)
```

---

## 9. Index Names

Indexes must use a prefix that identifies the index type.

| Prefix | Index Type | Pattern | Example |
|---|---|---|---|
| `ix_` | Nonclustered index | `ix_<table>_<column_or_purpose>` | `ix_fact_sales_order_date_key` |
| `ux_` | Unique index | `ux_<table>_<column_or_purpose>` | `ux_dim_customers_customer_id` |
| `cix_` | Clustered index, if explicitly named | `cix_<table>_<column>` | `cix_fact_sales_order_key` |

### Examples

- `ix_dim_customers_customer_id`
- `ix_fact_sales_customer_key`
- `ix_fact_sales_product_key`
- `ix_fact_sales_order_date_key`
- `ux_dim_products_product_id`

> In SQL Server, a primary key commonly creates a clustered index. Explicitly use `cix_` only when creating a separate named clustered index or when the project requires fully explicit index naming.

---

## 10. Script and Documentation File Names

All SQL scripts and documentation files must use `snake_case`.

### Pattern

```text
<project>_<layer_or_schema>_<object>_<purpose>.<extension>
```

### SQL File Examples

- `northwind_stage_create_tables.sql`
- `northwind_stage_load_data.sql`
- `northwind_dds_dim_customers.sql`
- `northwind_dds_fact_sales.sql`
- `northwind_sale_fact_sales.sql`
- `northwind_usp_load_dds_full.sql`

### Documentation File Examples

- `naming_conventions.md`
- `data_model_overview.md`
- `etl_process_flow.md`
- `deployment_guide.md`

---

## 11. Northwind Naming Gallery

### Stage Layer

- `stage.northwind_customers`
- `stage.northwind_orders`
- `stage.northwind_order_details`
- `stage.northwind_products`
- `stage.northwind_employees`
- `stage.northwind_suppliers`
- `stage.northwind_categories`
- `stage.northwind_shippers`
- `stage.northwind_regions`
- `stage.northwind_territories`
- `stage.northwind_employee_territories`

### DDS Layer

#### Dimensions

- `dds.dim_customers`
- `dds.dim_products`
- `dds.dim_employees`
- `dds.dim_suppliers`
- `dds.dim_categories`
- `dds.dim_shippers`
- `dds.dim_dates`
- `dds.dim_regions`
- `dds.dim_territories`

#### Facts

- `dds.fact_orders`
- `dds.fact_order_details`
- `dds.fact_sales`

#### Bridge Tables

- `dds.bridge_employee_territories`

### Sales Data Mart

#### Dimensions

- `sale.dim_customers`
- `sale.dim_products`
- `sale.dim_employees`
- `sale.dim_dates`
- `sale.dim_shippers`

#### Facts

- `sale.fact_sales`
- `sale.fact_order_details`

#### Views

- `sale.vw_monthly_sales`
- `sale.vw_top_customers`
- `sale.vw_sales_by_country`
- `sale.vw_product_performance`

---

## 12. Good Patterns and Naming Traps

### Recommended

- Use lowercase English names.
- Use `snake_case` consistently.
- Use source-aligned naming in `stage`.
- Use business-aligned naming in `dds` and data marts.
- Use separate schemas for business data marts.
- Use `_key` for surrogate keys.
- Use `_id` for source or business identifiers.
- Use `dwh_` for technical metadata.
- Use `usp_` for stored procedures.
- Use standard prefixes for constraints and indexes.

### Avoid

- `CustomerName`
- `customerName`
- `customer-name`
- `customer name`
- `tbl_customer`
- `dim_customer_tbl`
- `data1`
- `value_x`
- `tmp_orders`
- Reserved words such as `order`, `group`, and `user`
- Repeating the schema domain unnecessarily, such as `sale.fact_sale_sales`

---

## Final Standard at a Glance

Use the following naming rules as the project-wide baseline:

- **Technical layers**: `stage`, `dds`
- **Data marts**: business-domain schemas such as `sale`
- **Style**: lowercase `snake_case`
- **Stage tables**: `<source_system>_<entity>`
- **DDS and data mart tables**: `<category>_<entity>`
- **Surrogate keys**: `<entity>_key`
- **Business/source keys**: `<entity>_id`
- **Technical metadata**: `dwh_<column_name>`
- **Views**: `vw_<entity_or_purpose>`
- **Stored procedures**: `usp_<action>_<layer_or_schema>_<entity>`
- **Constraints**: `pk_`, `fk_`, `uq_`, `ck_`, `df_`
- **Indexes**: `ix_`, `ux_`, `cix_`

Applying these conventions consistently keeps the Northwind warehouse readable, maintainable, extensible, and easier to use for collaborative development and analytical reporting.
