# 🃏 Quick Recall Cards

<div class="section-banner">
  <span class="banner-icon">⚡</span>
  <span class="banner-text">Test yourself! Click each card to reveal the answer. Try to recall before peeking. These cover the most commonly asked ETL and SQL interview topics.</span>
</div>

---

## 🔄 ETL Concepts — "What is it?"

<div class="recall-grid">
  <details class="recall-card">
    <summary><strong>What does ETL stand for?</strong></summary>
    <div class="recall-answer">
      <strong>Extract, Transform, Load.</strong> Extract data from sources, Transform (clean, validate, reshape), Load into target system (data warehouse).
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a staging area?</strong></summary>
    <div class="recall-answer">
      A <strong>temporary holding area</strong> where raw source data lands before transformations are applied. Data is in its original format here — no business rules applied yet.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is source-to-target mapping?</strong></summary>
    <div class="recall-answer">
      A document defining how data flows from source to target — which source columns map to which target columns, what transformations are applied, data types, and NULL handling rules.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is data reconciliation?</strong></summary>
    <div class="recall-answer">
      Comparing <strong>summary-level data</strong> (row counts, SUM, COUNT DISTINCT) between source and target to ensure completeness. If totals match, the data is reconciled.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a surrogate key?</strong></summary>
    <div class="recall-answer">
      A <strong>system-generated unique ID</strong> (usually a sequence number) created by the ETL process. Unlike natural keys, surrogate keys have no business meaning.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a natural key?</strong></summary>
    <div class="recall-answer">
      A key that comes from the <strong>business data</strong> — like a customer ID, account number, or SSN. It has inherent meaning in the source system.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a dimension table?</strong></summary>
    <div class="recall-answer">
      Contains <strong>descriptive attributes</strong> (customers, products, dates). Changes slowly. Examples: dim_customer, dim_product, dim_date.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a fact table?</strong></summary>
    <div class="recall-answer">
      Contains <strong>measurable events/transactions</strong> (orders, transactions). Grows rapidly. Has foreign keys to dimension tables. Examples: fact_sales, fact_transaction.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What is data lineage?</strong></summary>
    <div class="recall-answer">
      Tracing data from its <strong>origin in source systems through transformations to its final destination</strong> in the target. Helps debug data quality issues by showing where data came from.
    </div>
  </details>
  <details class="recall-card">
    <summary><strong>What are the 6 common ETL defects?</strong></summary>
    <div class="recall-answer">
      1. <strong>Missing records</strong> — dropped by JOINs or filters<br>
      2. <strong>Duplicate records</strong> — incremental load issues<br>
      3. <strong>Wrong transformation</strong> — business rule errors<br>
      4. <strong>Data truncation</strong> — column length mismatch<br>
      5. <strong>NULL handling errors</strong> — defaults not applied<br>
      6. <strong>Date/time issues</strong> — timezone or format errors
    </div>
  </details>
</div>

---

## 💾 SQL Concepts — "How does it work?"

<div class="recall-grid">
  <details class="recall-card">
    <summary><strong>What does INNER JOIN return?</strong></summary>
    <div class="recall-answer">Only rows that have <strong>matching values in both tables</strong>. If no match, the row is excluded from the result.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does LEFT JOIN return?</strong></summary>
    <div class="recall-answer"><strong>All rows from the left table</strong> plus matching rows from the right. If no match, right-side columns are NULL. Use for finding <strong>missing records</strong>.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does FULL OUTER JOIN return?</strong></summary>
    <div class="recall-answer"><strong>All rows from both tables.</strong> NULLs where there's no match. Shows mismatches in <strong>both directions</strong>.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does GROUP BY do?</strong></summary>
    <div class="recall-answer">Groups rows with the <strong>same values</strong> in specified columns and lets you apply aggregate functions (COUNT, SUM, AVG, MIN, MAX) to each group.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What's the difference between WHERE and HAVING?</strong></summary>
    <div class="recall-answer"><strong>WHERE</strong> filters individual rows <strong>before</strong> aggregation. <strong>HAVING</strong> filters groups <strong>after</strong> aggregation. You can't use aggregate functions in WHERE.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does COALESCE do?</strong></summary>
    <div class="recall-answer">Returns the <strong>first non-NULL value</strong> from a list of arguments. Example: <code>COALESCE(phone, mobile, 'N/A')</code> returns whichever is not NULL first.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does ROW_NUMBER() do?</strong></summary>
    <div class="recall-answer">Assigns a <strong>unique sequential number</strong> to each row within a partition. No ties — always unique. Used for finding duplicates and getting the "latest" record.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What's the difference between RANK and DENSE_RANK?</strong></summary>
    <div class="recall-answer"><strong>RANK:</strong> Same rank for ties, then <strong>skips</strong> numbers (1, 2, 2, 4). <strong>DENSE_RANK:</strong> Same rank for ties, <strong>no skips</strong> (1, 2, 2, 3).</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does LAG() do?</strong></summary>
    <div class="recall-answer">Accesses a value from the <strong>previous row</strong> within the partition. <code>LAG(amount, 1)</code> gets the amount from one row before the current row.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a CTE?</strong></summary>
    <div class="recall-answer"><strong>Common Table Expression</strong> — a named temporary result set defined with <code>WITH</code>. Makes complex queries more readable. Only exists for the duration of that single query.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What is a correlated subquery?</strong></summary>
    <div class="recall-answer">A subquery that <strong>references columns from the outer query</strong> and runs once for each row of the outer query. Slower but sometimes necessary.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What does CASE WHEN do?</strong></summary>
    <div class="recall-answer">Acts as an <strong>if-then-else</strong> expression in SQL. Returns different values based on conditions. Used for deriving categories and implementing business rules.</div>
  </details>
</div>

---

## ⚖️ Key Differences — "What's the difference between…?"

<div class="recall-grid">
  <details class="recall-card">
    <summary><strong>ETL vs ELT</strong></summary>
    <div class="recall-answer"><strong>ETL:</strong> Transform in staging area before loading. <strong>ELT:</strong> Load raw data first, transform inside the target database. ELT is better for cloud platforms with large compute power.</div>
  </details>
  <details class="recall-card">
    <summary><strong>TRUNCATE vs DELETE</strong></summary>
    <div class="recall-answer"><strong>TRUNCATE:</strong> Removes all rows, fast, can't rollback, resets identity. <strong>DELETE:</strong> Can target specific rows with WHERE, slower, can rollback, doesn't reset identity.</div>
  </details>
  <details class="recall-card">
    <summary><strong>UNION vs UNION ALL</strong></summary>
    <div class="recall-answer"><strong>UNION:</strong> Combines results and <strong>removes duplicates</strong> (slower). <strong>UNION ALL:</strong> Combines results and <strong>keeps duplicates</strong> (faster). Use UNION ALL when you know there are no duplicates.</div>
  </details>
  <details class="recall-card">
    <summary><strong>Full Load vs Incremental Load</strong></summary>
    <div class="recall-answer"><strong>Full Load:</strong> All data extracted and loaded every time. <strong>Incremental:</strong> Only new/changed records since last run. Incremental is faster but more complex to test.</div>
  </details>
  <details class="recall-card">
    <summary><strong>SCD Type 1 vs Type 2</strong></summary>
    <div class="recall-answer"><strong>Type 1:</strong> Overwrite old data (no history). <strong>Type 2:</strong> Keep history with start/end dates and current flag. Type 2 is essential for audit trails.</div>
  </details>
  <details class="recall-card">
    <summary><strong>Surrogate Key vs Natural Key</strong></summary>
    <div class="recall-answer"><strong>Surrogate:</strong> System-generated, no business meaning (sequence number). <strong>Natural:</strong> From business data (account number, SSN). Surrogate keys are preferred in data warehouses.</div>
  </details>
  <details class="recall-card">
    <summary><strong>COUNT(*) vs COUNT(column)</strong></summary>
    <div class="recall-answer"><strong>COUNT(*):</strong> Counts <strong>all rows</strong> including NULLs. <strong>COUNT(column):</strong> Counts only <strong>non-NULL</strong> values in that column.</div>
  </details>
  <details class="recall-card">
    <summary><strong>IN vs EXISTS</strong></summary>
    <div class="recall-answer"><strong>IN:</strong> Better for small result sets, checks against a list. <strong>EXISTS:</strong> Better for large result sets, stops as soon as a match is found (short-circuit).</div>
  </details>
  <details class="recall-card">
    <summary><strong>Dimension Table vs Fact Table</strong></summary>
    <div class="recall-answer"><strong>Dimension:</strong> Descriptive attributes (WHO, WHAT, WHERE), changes slowly. <strong>Fact:</strong> Measurable events (HOW MUCH, HOW MANY), grows rapidly, has FK to dimensions.</div>
  </details>
  <details class="recall-card">
    <summary><strong>Clustered vs Non-Clustered Index</strong></summary>
    <div class="recall-answer"><strong>Clustered:</strong> Physically sorts table data, only ONE per table. <strong>Non-Clustered:</strong> Separate structure pointing to data, can have MANY per table.</div>
  </details>
</div>

---

## 🧪 Testing Concepts — "How do you test…?"

<div class="recall-grid">
  <details class="recall-card">
    <summary><strong>How do you find missing records?</strong></summary>
    <div class="recall-answer">Use <code>LEFT JOIN</code> from source to target on the business key, then <code>WHERE target_key IS NULL</code>. Or use <code>EXCEPT</code> to find rows in source not in target.</div>
  </details>
  <details class="recall-card">
    <summary><strong>How do you find duplicates?</strong></summary>
    <div class="recall-answer"><code>SELECT key_column, COUNT(*) FROM table GROUP BY key_column HAVING COUNT(*) > 1</code>. Or use <code>ROW_NUMBER()</code> partitioned by the key — row_num > 1 are duplicates.</div>
  </details>
  <details class="recall-card">
    <summary><strong>How do you check referential integrity?</strong></summary>
    <div class="recall-answer"><code>LEFT JOIN</code> fact table to dimension table on FK. <code>WHERE dimension_key IS NULL</code> finds orphan records — transactions with no matching account or customer.</div>
  </details>
  <details class="recall-card">
    <summary><strong>How do you validate a transformation?</strong></summary>
    <div class="recall-answer">JOIN source and target on the business key, then compare the source value against the expected transformed value. <code>WHERE target_col != EXPECTED_FUNCTION(source_col)</code>.</div>
  </details>
  <details class="recall-card">
    <summary><strong>How do you do data reconciliation?</strong></summary>
    <div class="recall-answer">Compare aggregate values: <code>SELECT COUNT(*), SUM(amount)</code> from both source and target. Use <code>UNION ALL</code> to see them side by side. Drill down by date or category if they don't match.</div>
  </details>
  <details class="recall-card">
    <summary><strong>What is regression testing in ETL?</strong></summary>
    <div class="recall-answer">Re-running existing test cases after a change or fix to ensure that <strong>nothing previously working is broken</strong>. Critical after ETL code changes.</div>
  </details>
</div>

---

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">💪 Practice until you can answer every card without peeking!</p>
