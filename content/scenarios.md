# 🎭 Scenario Labs

<div class="section-banner">
  <span class="banner-icon">🔥</span>
  <span class="banner-text">Real-world ETL testing scenarios. Walk through each step as if you're on the job. Try to think of the approach before revealing each step!</span>
</div>

---

<!-- Scenario 1 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">1</span>
    <span class="scenario-title">Row Count Mismatch Between Source and Target</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-red">
      🚨 <strong>Alert:</strong> After the nightly ETL load, the source system has 50,000 customer records but the target <code>DIM_CUSTOMER</code> table only has 48,200. 1,800 records are missing.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Confirm the mismatch</summary>
      <div class="step-content">
        Verify the counts independently:<br><br>
        <code>SELECT COUNT(*) FROM source_db.customers;</code><br>
        <code>SELECT COUNT(*) FROM target_db.dim_customer;</code><br><br>
        Difference: 50,000 - 48,200 = 1,800 missing records.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Identify the missing records</summary>
      <div class="step-content">
        Use LEFT JOIN to find records in source but not in target:<br><br>
        <code>SELECT s.customer_id, s.customer_name, s.status</code><br>
        <code>FROM source_db.customers s</code><br>
        <code>LEFT JOIN target_db.dim_customer t ON s.customer_id = t.customer_id</code><br>
        <code>WHERE t.customer_id IS NULL;</code><br><br>
        Look for <strong>patterns</strong> in the missing records — are they all from the same status, date range, or region?
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Investigate the root cause</summary>
      <div class="step-content">
        Common causes for missing records:<br><br>
        • <strong>NULL JOIN keys:</strong> Source records with NULL customer_id are dropped by INNER JOINs in the ETL<br>
        • <strong>Filter too restrictive:</strong> ETL has a WHERE clause excluding valid records<br>
        • <strong>Data type mismatch:</strong> Implicit conversion causing key mismatches<br>
        • <strong>Constraint violation:</strong> NOT NULL or unique constraints rejecting rows<br><br>
        Check the ETL error logs for rejected records.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 4: Document and report</summary>
      <div class="step-content">
        Raise a defect with:<br><br>
        • Source count: 50,000<br>
        • Target count: 48,200<br>
        • Missing: 1,800 records<br>
        • Root cause: [finding from Step 3]<br>
        • SQL query used for evidence<br>
        • Sample of affected records (5-10 examples)
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>LEFT JOIN to find missing records — the #1 ETL validation query</li>
      <li>Look for patterns in the missing data</li>
      <li>NULL JOIN keys are the most common cause</li>
      <li>Always document with SQL evidence</li>
    </ul>
  </div>
</div>

<!-- Scenario 2 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">2</span>
    <span class="scenario-title">Duplicate Records Found in Target Table</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-amber">
      ⚠️ <strong>Alert:</strong> The <code>DIM_CUSTOMER</code> table has 52,000 records but only 48,000 unique <code>customer_id</code> values. 4,000 duplicates exist.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Identify duplicate records</summary>
      <div class="step-content">
        <code>SELECT customer_id, COUNT(*) AS cnt</code><br>
        <code>FROM dim_customer</code><br>
        <code>GROUP BY customer_id</code><br>
        <code>HAVING COUNT(*) > 1</code><br>
        <code>ORDER BY cnt DESC;</code><br><br>
        This shows which customer_ids have duplicates and how many.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Examine the duplicate rows</summary>
      <div class="step-content">
        Pick a duplicate customer_id and look at all its records:<br><br>
        <code>SELECT * FROM dim_customer WHERE customer_id = 'CUST-12345' ORDER BY load_date;</code><br><br>
        Are they <strong>exact duplicates</strong> or <strong>near-duplicates</strong> with different load dates? This tells you if the ETL re-inserted records.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Determine the cause</summary>
      <div class="step-content">
        Common causes:<br><br>
        • <strong>Incremental load missing duplicate check:</strong> ETL inserts without checking if record exists<br>
        • <strong>Many-to-many JOIN:</strong> ETL query produces extra rows from unintended Cartesian product<br>
        • <strong>Multiple source systems:</strong> Same customer in two source systems creating two target records<br>
        • <strong>SCD Type 2 without current flag:</strong> Multiple versions without proper is_current management
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 4: Identify which records to keep</summary>
      <div class="step-content">
        Use ROW_NUMBER to identify duplicates for cleanup:<br><br>
        <code>WITH ranked AS (</code><br>
        <code>  SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY load_date DESC) AS rn</code><br>
        <code>  FROM dim_customer</code><br>
        <code>)</code><br>
        <code>SELECT * FROM ranked WHERE rn > 1;  -- These are the duplicates</code>
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>GROUP BY + HAVING COUNT(*) > 1 for finding duplicates</li>
      <li>ROW_NUMBER() for identifying which duplicate to keep</li>
      <li>Understanding incremental load issues</li>
      <li>Many-to-many JOIN awareness</li>
    </ul>
  </div>
</div>

<!-- Scenario 3 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">3</span>
    <span class="scenario-title">Data Transformation Logic Failure</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-red">
      🚨 <strong>Alert:</strong> In the target table, the <code>customer_tier</code> column shows 'Standard' for all customers, but the business rule says customers with balance > $50,000 should be 'Gold' and > $100,000 should be 'Platinum'.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Verify the business rule implementation</summary>
      <div class="step-content">
        Check what values exist in the target:<br><br>
        <code>SELECT customer_tier, COUNT(*) FROM dim_customer GROUP BY customer_tier;</code><br><br>
        Expected: distribution across Standard, Gold, Platinum. Actual: all 'Standard'.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Write the expected transformation query</summary>
      <div class="step-content">
        Based on the business rule:<br><br>
        <code>SELECT customer_id, total_balance,</code><br>
        <code>  CASE</code><br>
        <code>    WHEN total_balance >= 100000 THEN 'Platinum'</code><br>
        <code>    WHEN total_balance >= 50000 THEN 'Gold'</code><br>
        <code>    ELSE 'Standard'</code><br>
        <code>  END AS expected_tier,</code><br>
        <code>  customer_tier AS actual_tier</code><br>
        <code>FROM dim_customer</code><br>
        <code>WHERE total_balance >= 50000;</code><br><br>
        This shows records that should be Gold or Platinum but are showing as Standard.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Count the impact</summary>
      <div class="step-content">
        <code>SELECT</code><br>
        <code>  SUM(CASE WHEN total_balance >= 100000 THEN 1 ELSE 0 END) AS should_be_platinum,</code><br>
        <code>  SUM(CASE WHEN total_balance >= 50000 AND total_balance < 100000 THEN 1 ELSE 0 END) AS should_be_gold</code><br>
        <code>FROM dim_customer;</code><br><br>
        This quantifies the business impact — how many customers are mis-categorized.
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>CASE WHEN for transformation validation</li>
      <li>Understanding business rules and their SQL implementation</li>
      <li>Quantifying the impact of a defect</li>
      <li>Order of CASE conditions matters (check largest first)</li>
    </ul>
  </div>
</div>

<!-- Scenario 4 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">4</span>
    <span class="scenario-title">Incremental Load Missing New Records</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-amber">
      ⚠️ <strong>Alert:</strong> The daily incremental load ran successfully (no errors), but new customer registrations from yesterday are not appearing in the target table.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Verify new records exist in source</summary>
      <div class="step-content">
        <code>SELECT COUNT(*) FROM source_db.customers WHERE created_date = CAST(GETDATE()-1 AS DATE);</code><br><br>
        This confirms new records exist in the source system.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Check the incremental watermark</summary>
      <div class="step-content">
        Most incremental loads use a <strong>watermark</strong> (last loaded timestamp) to identify new records.<br><br>
        <code>SELECT MAX(load_date) AS last_load FROM target_db.dim_customer;</code><br><br>
        If the watermark is stale, the ETL may be looking at the wrong date range for new records. Common issue: timezone mismatch between source and ETL server.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Check the ETL extract query</summary>
      <div class="step-content">
        The ETL might be using <code>WHERE created_date > @last_load_date</code>. If the source uses a different date format or timezone, new records could be excluded.<br><br>
        Also check: Is the ETL looking at <code>created_date</code> or <code>modified_date</code>? New records only have a created_date — if the ETL filters on modified_date, new records are missed.
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>Understanding incremental load watermarks</li>
      <li>Timezone and date format awareness</li>
      <li>Difference between created_date and modified_date</li>
      <li>ETL can succeed technically but fail functionally</li>
    </ul>
  </div>
</div>

<!-- Scenario 5 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">5</span>
    <span class="scenario-title">NULL Values Where NOT NULL Expected</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-amber">
      ⚠️ <strong>Alert:</strong> The <code>email</code> column in <code>DIM_CUSTOMER</code> has 5,000 NULL values, but the mapping document says it should default to 'NOT_PROVIDED' when the source email is NULL.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Quantify the issue</summary>
      <div class="step-content">
        <code>SELECT COUNT(*) AS null_emails FROM dim_customer WHERE email IS NULL;</code><br>
        <code>SELECT COUNT(*) AS default_emails FROM dim_customer WHERE email = 'NOT_PROVIDED';</code><br><br>
        The NULL count should be 0 and the default count should be > 0 if the transformation is working.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Trace back to source</summary>
      <div class="step-content">
        Check if the source also has NULLs:<br><br>
        <code>SELECT COUNT(*) FROM source_db.customers WHERE email IS NULL;</code><br><br>
        If source has 5,000 NULLs and target has 5,000 NULLs, the COALESCE/ISNULL transformation is not being applied. The ETL is passing NULLs through without applying the default value.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Document the expected behavior</summary>
      <div class="step-content">
        Expected transformation: <code>COALESCE(source_email, 'NOT_PROVIDED')</code><br><br>
        Raise defect: "The COALESCE transformation for email column is not applied. 5,000 records have NULL emails instead of 'NOT_PROVIDED' per the mapping document."
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>NULL checking with IS NULL</li>
      <li>COALESCE for default value handling</li>
      <li>Tracing issues back through source-to-target mapping</li>
      <li>Clear defect documentation</li>
    </ul>
  </div>
</div>

<!-- Scenario 6 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">6</span>
    <span class="scenario-title">Date Format Mismatch Across Systems</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-red">
      🚨 <strong>Alert:</strong> Transaction dates in the target are wrong. A transaction from March 1st, 2024 shows as January 3rd, 2024 in the target.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Identify the pattern</summary>
      <div class="step-content">
        March 1 = 03/01/2024 → interpreted as January 3 = 01/03/2024<br><br>
        This is a classic <strong>MM/DD vs DD/MM</strong> date format mismatch. The source sends dates as DD/MM/YYYY but the ETL parses them as MM/DD/YYYY.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Find all affected records</summary>
      <div class="step-content">
        Records where day ≤ 12 are affected (because they can be misinterpreted). Records where day > 12 may fail or be correct depending on error handling.<br><br>
        <code>SELECT s.txn_date AS source_date, t.txn_date AS target_date</code><br>
        <code>FROM source_db.transactions s</code><br>
        <code>INNER JOIN target_db.fact_transaction t ON s.txn_id = t.txn_id</code><br>
        <code>WHERE s.txn_date != t.txn_date</code><br>
        <code>ORDER BY s.txn_date;</code>
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>DD/MM vs MM/DD date format awareness</li>
      <li>Pattern recognition in data quality issues</li>
      <li>Impact of ambiguous date formats on financial reporting</li>
    </ul>
  </div>
</div>

<!-- Scenario 7 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">7</span>
    <span class="scenario-title">Referential Integrity Violation in Target</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-red">
      🚨 <strong>Alert:</strong> The <code>FACT_TRANSACTION</code> table has 500 records with <code>account_id</code> values that don't exist in <code>DIM_ACCOUNT</code>.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Find the orphan records</summary>
      <div class="step-content">
        <code>SELECT t.transaction_id, t.account_id, t.amount</code><br>
        <code>FROM fact_transaction t</code><br>
        <code>LEFT JOIN dim_account a ON t.account_id = a.account_id</code><br>
        <code>WHERE a.account_id IS NULL;</code><br><br>
        This gives you all transactions pointing to non-existent accounts.
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Determine loading order</summary>
      <div class="step-content">
        Referential integrity violations usually happen when:<br><br>
        • <strong>Fact table loaded before dimension:</strong> Transactions loaded before accounts<br>
        • <strong>Dimension filtering:</strong> Inactive accounts excluded from DIM_ACCOUNT but their transactions still loaded<br>
        • <strong>New accounts:</strong> Account created same day as transaction but dimension load ran first
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Check if accounts exist in source</summary>
      <div class="step-content">
        <code>SELECT DISTINCT t.account_id</code><br>
        <code>FROM fact_transaction t</code><br>
        <code>LEFT JOIN dim_account a ON t.account_id = a.account_id</code><br>
        <code>WHERE a.account_id IS NULL</code><br><br>
        Then check: <code>SELECT * FROM source_db.accounts WHERE account_id IN (list from above);</code><br><br>
        If they exist in the source but not the target dimension, the dimension ETL has a bug.
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>LEFT JOIN for referential integrity checking</li>
      <li>Understanding ETL load order (dimensions before facts)</li>
      <li>Orphan record investigation</li>
      <li>Tracing issues back to source systems</li>
    </ul>
  </div>
</div>

<!-- Scenario 8 -->
<div class="scenario-box">
  <div class="scenario-header">
    <span class="scenario-num">8</span>
    <span class="scenario-title">Aggregate Mismatch — Amount Totals Don't Match</span>
  </div>
  <div class="scenario-body">
    <div class="scenario-alert alert-red">
      🚨 <strong>Alert:</strong> Daily transaction total in source is $1,234,567.89 but target shows $1,234,467.89. There's a $100 difference.
    </div>
    <details class="scenario-step">
      <summary>📋 Step 1: Confirm the mismatch</summary>
      <div class="step-content">
        <code>SELECT 'Source' AS sys, SUM(amount) AS total FROM source_db.transactions WHERE txn_date = '2024-01-15'</code><br>
        <code>UNION ALL</code><br>
        <code>SELECT 'Target' AS sys, SUM(amount) AS total FROM target_db.fact_transaction WHERE txn_date = '2024-01-15';</code>
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 2: Drill down by category</summary>
      <div class="step-content">
        Break down by transaction type or category to isolate where the difference is:<br><br>
        <code>SELECT txn_type, SUM(s_amt) AS source_total, SUM(t_amt) AS target_total, SUM(s_amt) - SUM(t_amt) AS diff</code><br>
        <code>FROM (</code><br>
        <code>  SELECT s.txn_type, s.amount AS s_amt, COALESCE(t.amount, 0) AS t_amt</code><br>
        <code>  FROM source_db.transactions s</code><br>
        <code>  LEFT JOIN target_db.fact_transaction t ON s.txn_id = t.txn_id</code><br>
        <code>  WHERE s.txn_date = '2024-01-15'</code><br>
        <code>) sub GROUP BY txn_type HAVING SUM(s_amt) != SUM(t_amt);</code>
      </div>
    </details>
    <details class="scenario-step">
      <summary>📋 Step 3: Check for precision issues</summary>
      <div class="step-content">
        Common cause: <strong>FLOAT vs DECIMAL</strong> data type mismatch.<br><br>
        FLOAT can introduce tiny rounding errors (e.g., $100.00 stored as $99.9999998). If many transactions have these micro-differences, they add up to $100.<br><br>
        Check: <code>SELECT data_type FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name = 'fact_transaction' AND column_name = 'amount';</code>
      </div>
    </details>
  </div>
  <div class="scenario-debrief">
    <div class="debrief-title">✅ Key Concepts Tested</div>
    <ul class="debrief-list">
      <li>Data reconciliation with SUM comparison</li>
      <li>Drill-down approach to isolate differences</li>
      <li>FLOAT vs DECIMAL precision — critical in banking</li>
      <li>Using COALESCE in reconciliation queries</li>
    </ul>
  </div>
</div>

---

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">🎯 Practice talking through these scenarios out loud — that's how interviews work!</p>
