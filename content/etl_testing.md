# 🧪 ETL Testing — Deep Dive

<div class="section-banner">
  <span class="banner-icon">🔬</span>
  <span class="banner-text">This section covers the ETL testing lifecycle, test design from mapping documents, and real-world defect patterns from banking and retail domains — the kind of experience your interviewer expects from 6+ years in ETL testing.</span>
</div>

---

## ETL Testing Lifecycle

```mermaid
flowchart LR
    A["📋 Understand<br/>Requirements"] --> B["📄 Review<br/>Mapping Docs"]
    B --> C["✏️ Design<br/>Test Cases"]
    C --> D["🔄 Execute<br/>Tests"]
    D --> E["🐛 Log<br/>Defects"]
    E --> F["✅ Retest<br/>& Sign-off"]
```

### Phase 1: Understand Requirements
- Review Business Requirement Documents (BRDs) and Functional Requirement Documents (FRDs)
- Understand the data sources, transformation logic, and target schema
- Identify critical business rules (e.g., "inactive accounts should be excluded")

### Phase 2: Review Source-to-Target Mapping
- The **mapping document** is the single most important artifact for an ETL tester
- It defines: source table → source column → transformation → target column
- Verify: data types, default values, NULL handling, business rules

### Phase 3: Design Test Cases

| Test Category | What You Test | Example |
|--------------|---------------|---------|
| **Row Count** | Record counts match at each layer | Source: 10,000 → Target: 10,000 |
| **Data Completeness** | No critical data is lost | All NOT NULL columns are populated |
| **Data Accuracy** | Values are correct after transformation | Date format conversion is accurate |
| **Duplicate Check** | No unintended duplicates | Unique customer IDs in target |
| **Referential Integrity** | FK relationships are valid | Every transaction links to a valid account |
| **Boundary Testing** | Edge cases handled | Max varchar length, zero amounts, future dates |
| **Negative Testing** | Invalid data is rejected | Alphabetic characters in numeric fields |
| **Incremental Load** | Only new/changed records loaded | Yesterday's data not re-inserted |

### Phase 4: Execute Tests
- Write SQL queries to validate data at each layer
- Compare source data against target data
- Document results with screenshots and query outputs

### Phase 5: Log Defects
- Clear defect title: "Missing 45 records in target table DIM_CUSTOMER"
- Include: source count, target count, sample mismatched records, SQL query used
- Severity levels: Critical (data loss), Major (incorrect transformation), Minor (formatting)

### Phase 6: Retest & Sign-off
- Verify defect fixes didn't introduce new issues (regression testing)
- Complete test summary report
- Obtain stakeholder sign-off

---

## Source-to-Target Mapping — How to Read and Validate

A typical mapping document looks like this:

| Source Table | Source Column | Transformation | Target Table | Target Column | Data Type | Nullable |
|-------------|--------------|----------------|-------------|--------------|-----------|----------|
| CRM.CUSTOMERS | FIRST_NAME | UPPER() | DW.DIM_CUSTOMER | CUST_FIRST_NAME | VARCHAR(100) | N |
| CRM.CUSTOMERS | LAST_NAME | UPPER() | DW.DIM_CUSTOMER | CUST_LAST_NAME | VARCHAR(100) | N |
| CRM.CUSTOMERS | DOB | Convert to YYYY-MM-DD | DW.DIM_CUSTOMER | DATE_OF_BIRTH | DATE | Y |
| CRM.CUSTOMERS | STATUS_CD | Lookup: A→Active, I→Inactive | DW.DIM_CUSTOMER | CUSTOMER_STATUS | VARCHAR(20) | N |
| — | — | Generate sequence | DW.DIM_CUSTOMER | CUSTOMER_SK | INT | N |

### Validation Queries from the Mapping Above

```sql
-- Verify UPPER transformation
SELECT s.FIRST_NAME, t.CUST_FIRST_NAME
FROM CRM.CUSTOMERS s
INNER JOIN DW.DIM_CUSTOMER t ON s.CUSTOMER_ID = t.CUSTOMER_ID
WHERE t.CUST_FIRST_NAME != UPPER(s.FIRST_NAME);

-- Verify status lookup
SELECT s.STATUS_CD, t.CUSTOMER_STATUS
FROM CRM.CUSTOMERS s
INNER JOIN DW.DIM_CUSTOMER t ON s.CUSTOMER_ID = t.CUSTOMER_ID
WHERE (s.STATUS_CD = 'A' AND t.CUSTOMER_STATUS != 'Active')
   OR (s.STATUS_CD = 'I' AND t.CUSTOMER_STATUS != 'Inactive');

-- Verify surrogate key uniqueness
SELECT CUSTOMER_SK, COUNT(*) FROM DW.DIM_CUSTOMER
GROUP BY CUSTOMER_SK HAVING COUNT(*) > 1;
```

---

## Incremental Load vs Full Load Testing

### Full Load
- **All** records are extracted and loaded every time
- Target table is typically truncated before loading
- **Test focus:** Row count, data accuracy, complete refresh

### Incremental Load
- Only **new or changed** records since the last run are loaded
- Uses timestamps, change flags, or Change Data Capture (CDC)
- **Test focus:** New records appear, changed records update, unchanged records stay the same, deleted records handled correctly

```sql
-- Verify incremental load: new records since last run
SELECT COUNT(*) AS new_records
FROM DW.DIM_CUSTOMER
WHERE load_date = CAST(GETDATE() AS DATE)
  AND created_date >= '2024-01-15';  -- last ETL run date

-- Verify no duplicate inserts from incremental load
SELECT customer_id, COUNT(*) AS cnt
FROM DW.DIM_CUSTOMER
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

---

## Common ETL Defect Patterns

### 1. Missing Records
**Symptom:** Target has fewer rows than source
**Common causes:**
- Filter condition too restrictive
- JOIN dropping unmatched rows (using INNER JOIN instead of LEFT JOIN)
- NULL values in JOIN keys causing records to be excluded
- Constraint violations rejecting rows silently

### 2. Duplicate Records
**Symptom:** Target has more rows than expected
**Common causes:**
- Missing DISTINCT in the source query
- Many-to-many JOIN producing extra rows
- Incremental load re-inserting existing records (missing duplicate check)

### 3. Wrong Transformation
**Symptom:** Values in target don't match expected transformation
**Common causes:**
- Incorrect business rule implementation
- Data type precision loss (FLOAT vs DECIMAL)
- Timezone conversion errors
- Locale-specific date/number formatting

### 4. Data Truncation
**Symptom:** Values are cut off in target
**Common causes:**
- Target column too short (e.g., VARCHAR(50) vs VARCHAR(200) in source)
- Silent truncation of long strings

### 5. NULL Handling Errors
**Symptom:** NULLs where values expected, or wrong defaults
**Common causes:**
- Source NULLs not converted to default values
- COALESCE or ISNULL not applied
- NULL JOIN keys not handled

### 6. Date/Time Issues
**Symptom:** Wrong dates, timezone shifts, format errors
**Common causes:**
- Timezone not converted (UTC vs local)
- Date format mismatch between source systems
- Daylight saving time edge cases

---

## Banking Domain ETL Examples

### Account Balance Reconciliation

```sql
-- Compare daily balance from transactions vs balance table
WITH calculated_balance AS (
    SELECT
        account_id,
        SUM(CASE WHEN txn_type = 'Credit' THEN amount ELSE -amount END) AS calc_balance
    FROM fact_transaction
    WHERE txn_date <= '2024-01-15'
    GROUP BY account_id
)
SELECT
    a.account_id,
    a.current_balance,
    c.calc_balance,
    a.current_balance - c.calc_balance AS difference
FROM dim_account a
INNER JOIN calculated_balance c ON a.account_id = c.account_id
WHERE ABS(a.current_balance - c.calc_balance) > 0.01;
```

### Transaction Categorization Validation

```sql
-- Verify transaction type mapping
SELECT txn_code, txn_description, txn_category
FROM fact_transaction
WHERE (txn_code IN ('DEP', 'CASH_DEP', 'CHK_DEP') AND txn_category != 'Deposit')
   OR (txn_code IN ('WTH', 'ATM_WTH') AND txn_category != 'Withdrawal')
   OR (txn_code IN ('TRF_IN', 'TRF_OUT') AND txn_category != 'Transfer');
```

---

## Retail Domain ETL Examples

### Inventory Reconciliation

```sql
-- Compare POS system sales with warehouse inventory adjustments
SELECT
    p.product_id,
    p.product_name,
    inv.warehouse_qty,
    sales.total_sold,
    inv.warehouse_qty + sales.total_sold AS expected_opening_qty
FROM dim_product p
INNER JOIN (SELECT product_id, SUM(qty) AS warehouse_qty FROM fact_inventory GROUP BY product_id) inv ON p.product_id = inv.product_id
INNER JOIN (SELECT product_id, SUM(qty_sold) AS total_sold FROM fact_sales WHERE sale_date = '2024-01-15' GROUP BY product_id) sales ON p.product_id = sales.product_id;
```

### Product Hierarchy Validation

```sql
-- Verify category → subcategory → product hierarchy
SELECT p.product_id, p.product_name, p.subcategory_id, sc.category_id
FROM dim_product p
LEFT JOIN dim_subcategory sc ON p.subcategory_id = sc.subcategory_id
WHERE sc.subcategory_id IS NULL;  -- Orphan products
```

---

## Business Rule Validation Strategies

### Strategy 1: Positive Testing
Verify that valid data is processed correctly:
- Valid records appear in target
- Transformations produce expected results
- Lookups return correct mapped values

### Strategy 2: Negative Testing
Verify that invalid data is handled properly:
- Invalid records are rejected or quarantined
- Error logging captures the issue
- No partial records in target tables

### Strategy 3: Boundary Testing
Test edge cases:
- Maximum length strings
- Zero and negative amounts
- Dates at year boundaries (Dec 31 → Jan 1)
- Special characters and Unicode
- Empty strings vs NULLs

---

## Performance Testing Concepts

While not typically the ETL tester's primary focus, be aware of:

- **Volume testing:** Can the ETL handle peak data volumes?
- **Timing:** Does the ETL complete within the batch window?
- **Resource usage:** CPU, memory, disk I/O during execution
- **Bottleneck identification:** Which stage is slowest — extract, transform, or load?

> **Interview tip:** You probably won't be asked to performance test, but saying "I've observed slow-running ETL jobs and investigated whether the bottleneck was in the extract, transform, or load phase" shows experience.

---

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">🧪 ETL Testing is all about attention to detail — every record counts!</p>
