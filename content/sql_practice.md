# ⚡ SQL Practice — Query Templates for ETL Testing

<div class="section-banner">
  <span class="banner-icon">💾</span>
  <span class="banner-text">Ready-to-use SQL query templates organized by ETL testing task. Copy, adapt, and practice these — they represent the exact type of queries you'll write on the job and discuss in interviews.</span>
</div>

---

> ⚠️ **SQL Dialect Note:** The queries below use **SQL Server** syntax by default (e.g., `GETDATE()`, `LEN()`, `TRY_CAST()`). Your resume spans Oracle, SQL Server, and MySQL. Key equivalents:
>
> | Function | SQL Server | Oracle | MySQL |
> |----------|-----------|--------|-------|
> | Current date/time | `GETDATE()` | `SYSDATE` | `NOW()` |
> | String length | `LEN(s)` | `LENGTH(s)` | `LENGTH(s)` |
> | Safe cast | `TRY_CAST(x AS type)` | *(no direct equivalent)* | *(no direct equivalent)* |
> | Add days | `DATEADD(day, n, date)` | `date + n` | `DATE_ADD(date, INTERVAL n DAY)` |
> | Top N rows | `SELECT TOP N ...` | `WHERE ROWNUM <= N` / `FETCH FIRST N ROWS ONLY` | `LIMIT N` |
>

## 1. Row Count Validation

```sql
-- Compare source and target row counts
SELECT 'Source' AS system, COUNT(*) AS row_count FROM source_db.customers
UNION ALL
SELECT 'Target' AS system, COUNT(*) AS row_count FROM target_db.dim_customer;
```

```sql
-- Row count by date partition
SELECT txn_date, COUNT(*) AS record_count
FROM fact_transaction
GROUP BY txn_date
ORDER BY txn_date DESC;
```

---

## 2. Finding Missing Records (Source vs Target)

```sql
-- Records in source but NOT in target
SELECT s.customer_id, s.customer_name
FROM source_db.customers s
LEFT JOIN target_db.dim_customer t ON s.customer_id = t.customer_id
WHERE t.customer_id IS NULL;
```

```sql
-- Using EXCEPT (records in source missing from target)
SELECT customer_id FROM source_db.customers
EXCEPT
SELECT customer_id FROM target_db.dim_customer;
```

---

## 3. Duplicate Detection

```sql
-- Find duplicate records by key column
SELECT customer_id, COUNT(*) AS cnt
FROM dim_customer
GROUP BY customer_id
HAVING COUNT(*) > 1;
```

```sql
-- Find exact duplicate rows (all columns match)
SELECT customer_id, customer_name, email, COUNT(*) AS duplicates
FROM dim_customer
GROUP BY customer_id, customer_name, email
HAVING COUNT(*) > 1;
```

```sql
-- Remove duplicates keeping the latest record using ROW_NUMBER
WITH ranked AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY load_date DESC) AS rn
    FROM dim_customer
)
SELECT * FROM ranked WHERE rn > 1;  -- These are the duplicates to investigate
```

---

## 4. NULL and Blank Checking

```sql
-- Check for NULLs in mandatory columns
SELECT
    SUM(CASE WHEN customer_name IS NULL THEN 1 ELSE 0 END) AS null_names,
    SUM(CASE WHEN email IS NULL THEN 1 ELSE 0 END) AS null_emails,
    SUM(CASE WHEN phone IS NULL THEN 1 ELSE 0 END) AS null_phones,
    COUNT(*) AS total_records
FROM dim_customer;
```

```sql
-- Check for empty strings AND NULLs
SELECT customer_id, customer_name
FROM dim_customer
WHERE customer_name IS NULL
   OR LTRIM(RTRIM(customer_name)) = '';
```

---

## 5. Data Transformation Verification

```sql
-- Verify UPPER case transformation
SELECT s.first_name AS source_val, t.first_name AS target_val
FROM source_db.customers s
INNER JOIN target_db.dim_customer t ON s.customer_id = t.customer_id
WHERE t.first_name != UPPER(s.first_name)
   OR t.first_name IS NULL;
```

```sql
-- Verify date format conversion
SELECT s.date_of_birth AS source_date, t.dob AS target_date
FROM source_db.customers s
INNER JOIN target_db.dim_customer t ON s.customer_id = t.customer_id
WHERE CAST(s.date_of_birth AS DATE) != t.dob;
```

```sql
-- Verify derived column (full name = first + last)
SELECT customer_id, first_name, last_name, full_name
FROM dim_customer
WHERE full_name != CONCAT(first_name, ' ', last_name);
```

---

## 6. Aggregate Comparison (Data Reconciliation)

```sql
-- Compare totals between source and target
SELECT 'Source' AS system, SUM(amount) AS total_amount, COUNT(*) AS txn_count
FROM source_db.transactions WHERE txn_date = '2024-01-15'
UNION ALL
SELECT 'Target' AS system, SUM(amount) AS total_amount, COUNT(*) AS txn_count
FROM target_db.fact_transaction WHERE txn_date = '2024-01-15';
```

```sql
-- Reconcile by category
SELECT
    s.category,
    s.source_total,
    t.target_total,
    s.source_total - t.target_total AS difference
FROM (SELECT category, SUM(amount) AS source_total FROM source_db.transactions GROUP BY category) s
FULL OUTER JOIN (SELECT category, SUM(amount) AS target_total FROM target_db.fact_transaction GROUP BY category) t
ON s.category = t.category
WHERE ABS(COALESCE(s.source_total, 0) - COALESCE(t.target_total, 0)) > 0.01;
```

---

## 7. Referential Integrity Checks

```sql
-- Find orphan records (FK has no matching PK)
SELECT t.transaction_id, t.account_id
FROM fact_transaction t
LEFT JOIN dim_account a ON t.account_id = a.account_id
WHERE a.account_id IS NULL;
```

```sql
-- Check all foreign key relationships at once
SELECT 'Orphan Transactions' AS issue, COUNT(*) AS cnt
FROM fact_transaction t LEFT JOIN dim_account a ON t.account_id = a.account_id WHERE a.account_id IS NULL
UNION ALL
SELECT 'Orphan Orders' AS issue, COUNT(*) AS cnt
FROM fact_orders o LEFT JOIN dim_customer c ON o.customer_id = c.customer_id WHERE c.customer_id IS NULL
UNION ALL
SELECT 'Orphan Line Items' AS issue, COUNT(*) AS cnt
FROM fact_order_lines l LEFT JOIN dim_product p ON l.product_id = p.product_id WHERE p.product_id IS NULL;
```

---

## 8. Date Range Validation

```sql
-- Ensure no future dates in historical data
SELECT * FROM fact_transaction
WHERE txn_date > GETDATE();   -- SQL Server; use SYSDATE (Oracle) or NOW() (MySQL)

-- Ensure no dates before business start
SELECT * FROM fact_transaction
WHERE txn_date < '2010-01-01';

-- Check for date gaps in daily data
WITH date_series AS (
    SELECT DISTINCT CAST(txn_date AS DATE) AS txn_date
    FROM fact_transaction
    WHERE txn_date BETWEEN '2024-01-01' AND '2024-01-31'
)
SELECT DATEADD(day, 1, d1.txn_date) AS missing_date   -- SQL Server; Oracle: d1.txn_date + 1; MySQL: DATE_ADD(d1.txn_date, INTERVAL 1 DAY)
FROM date_series d1
LEFT JOIN date_series d2 ON d2.txn_date = DATEADD(day, 1, d1.txn_date)  -- same adaptation as above
WHERE d2.txn_date IS NULL
  AND d1.txn_date < '2024-01-31';
```

---

## 9. Data Type and Length Validation

```sql
-- Find values that exceed expected length
SELECT customer_id, customer_name, LEN(customer_name) AS name_length  -- SQL Server; use LENGTH() in Oracle/MySQL
FROM dim_customer
WHERE LEN(customer_name) > 100;  -- SQL Server; use LENGTH() in Oracle/MySQL

-- Find non-numeric values in a "numeric" column stored as varchar
SELECT account_number
FROM dim_account
WHERE TRY_CAST(account_number AS BIGINT) IS NULL  -- SQL Server-specific; no direct equivalent in Oracle/MySQL
  AND account_number IS NOT NULL;
```

---

## 10. Incremental Load Validation

```sql
-- Verify new records were inserted
SELECT COUNT(*) AS new_records
FROM dim_customer
WHERE load_date = CAST(GETDATE() AS DATE);  -- SQL Server; use TRUNC(SYSDATE) (Oracle) or CURDATE() (MySQL)

-- Verify updated records were modified, not re-inserted
SELECT customer_id, COUNT(*) AS versions
FROM dim_customer
WHERE modified_date >= '2024-01-15'
GROUP BY customer_id
HAVING COUNT(*) > 1;  -- Should be 1 if update-in-place (SCD Type 1)
```

---

## 11. SCD Type 2 Validation

```sql
-- Verify only one active record per customer
SELECT customer_id, COUNT(*) AS active_records
FROM dim_customer
WHERE is_current = 'Y'
GROUP BY customer_id
HAVING COUNT(*) > 1;  -- Should never happen

-- Verify date continuity (no gaps between versions)
SELECT
    a.customer_id,
    a.effective_end_date AS prev_end,
    b.effective_start_date AS next_start
FROM dim_customer a
INNER JOIN dim_customer b ON a.customer_id = b.customer_id
    AND a.effective_end_date < b.effective_start_date
    AND NOT EXISTS (
        SELECT 1 FROM dim_customer c
        WHERE c.customer_id = a.customer_id
          AND c.effective_start_date > a.effective_start_date
          AND c.effective_start_date < b.effective_start_date
    )
WHERE DATEDIFF(day, a.effective_end_date, b.effective_start_date) > 1;
```

---

## 12. Cross-System Data Comparison

```sql
-- Side-by-side comparison of key columns
SELECT
    s.customer_id,
    s.customer_name AS source_name,
    t.customer_name AS target_name,
    CASE WHEN s.customer_name = t.customer_name THEN 'Match' ELSE 'MISMATCH' END AS name_check,
    s.email AS source_email,
    t.email AS target_email,
    CASE WHEN COALESCE(s.email, '') = COALESCE(t.email, '') THEN 'Match' ELSE 'MISMATCH' END AS email_check
FROM source_db.customers s
INNER JOIN target_db.dim_customer t ON s.customer_id = t.customer_id
WHERE s.customer_name != t.customer_name
   OR COALESCE(s.email, '') != COALESCE(t.email, '');
```

---

## 13. Summary Statistics for Quick Data Profiling

```sql
-- Quick data profile of a table
SELECT
    COUNT(*) AS total_rows,
    COUNT(DISTINCT customer_id) AS unique_customers,
    MIN(created_date) AS earliest_record,
    MAX(created_date) AS latest_record,
    SUM(CASE WHEN email IS NULL THEN 1 ELSE 0 END) AS null_emails,
    SUM(CASE WHEN phone IS NULL THEN 1 ELSE 0 END) AS null_phones,
    COUNT(DISTINCT state) AS unique_states
FROM dim_customer;
```

---

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">⚡ Practice writing these queries from memory — that's the best interview preparation!</p>
