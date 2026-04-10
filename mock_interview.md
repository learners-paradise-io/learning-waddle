# 🎤 Mock Interview — 50 Questions & Answers

<div class="section-banner">
  <span class="banner-icon">🎯</span>
  <span class="banner-text">Practice like it's the real thing. Read each question, form your answer in your head (or say it out loud!), THEN click to reveal the model answer. Don't just read — practice responding.</span>
</div>

---

## 🔄 ETL Testing Questions (15)

<details>
<summary><strong>Q1:</strong> What is ETL and what role does an ETL tester play?</summary>

**Answer:** ETL stands for Extract, Transform, Load. It's the process of pulling data from source systems, transforming it (cleaning, validating, reshaping), and loading it into a target system like a data warehouse. As an ETL tester, my role is to validate every stage — ensuring row counts match, transformations are correct, no data is lost, no duplicates are created, and referential integrity is maintained. I do this primarily by writing SQL queries to compare source and target data.

</details>

<details>
<summary><strong>Q2:</strong> What is a source-to-target mapping document and how do you use it?</summary>

**Answer:** A source-to-target mapping defines exactly how data flows from source to target — which source columns map to which target columns, what transformations are applied, the target data types, and NULL handling rules. As a tester, it's my primary reference document. I create test cases from each mapping rule and write SQL queries to verify that every transformation defined in the mapping is applied correctly in the target data.

</details>

<details>
<summary><strong>Q3:</strong> How do you validate data after an ETL load?</summary>

**Answer:** I follow a systematic approach: First, I validate row counts — source versus target. Then I check for duplicates using GROUP BY and HAVING. Next, I verify transformations by comparing source values against expected target values. I check referential integrity using LEFT JOINs to find orphan records. Finally, I do aggregate reconciliation — comparing SUM and COUNT between source and target. If any step finds a mismatch, I drill down to identify affected records and root cause.

</details>

<details>
<summary><strong>Q4:</strong> What is the difference between ETL and ELT?</summary>

**Answer:** In ETL, data is transformed in a staging area before being loaded into the target. In ELT, raw data is loaded directly into the target, and transformations happen inside the target database. ELT is more common in cloud platforms like Snowflake and BigQuery where the target has massive compute power. The key difference is WHERE the transformation happens.

</details>

<details>
<summary><strong>Q5:</strong> What is an incremental load and how do you test it?</summary>

**Answer:** An incremental load only processes records that are new or changed since the last ETL run, using timestamps or change flags. When testing, I verify: new records appear in the target, changed records are updated correctly, unchanged records are NOT modified, and no duplicate records are created. The biggest trap to test for is duplicate inserts — where the ETL re-inserts existing records because the duplicate check is missing.

</details>

<details>
<summary><strong>Q6:</strong> What is data reconciliation?</summary>

**Answer:** Data reconciliation means comparing summary-level data between source and target to ensure completeness. I compare row counts, SUM of amounts, and COUNT DISTINCT of key columns. If source has 10,000 transactions totaling $5 million and the target shows the same, the load is reconciled. If there's a difference, I drill down by date, category, or account to isolate where the mismatch is.

</details>

<details>
<summary><strong>Q7:</strong> What are the most common ETL defects you've seen?</summary>

**Answer:** The most common defects are: missing records — usually caused by NULL JOIN keys being dropped; duplicate records — from incremental loads not checking for existing records; wrong transformations — business rules not implemented correctly; data truncation — target column shorter than source; NULL handling errors — defaults not applied; and date format mismatches between source systems.

</details>

<details>
<summary><strong>Q8:</strong> What is a staging area?</summary>

**Answer:** The staging area is a temporary holding zone between source and target systems. Raw data from sources lands here first, in its original format — no business rules are applied yet. It serves as a checkpoint where you can validate data before transformations. If something goes wrong in the ETL, you can go back to staging without re-extracting from source.

</details>

<details>
<summary><strong>Q9:</strong> What is SCD Type 1 versus Type 2?</summary>

**Answer:** SCD Type 1 simply overwrites the old record with new data — no history is kept. SCD Type 2 creates a new row with the updated data, keeping the old row with start/end dates and a current flag. Type 2 preserves complete history, which is critical in banking for audit purposes. When testing Type 2, I verify only one record per entity has is_current = 'Y', and there are no date gaps between versions.

</details>

<details>
<summary><strong>Q10:</strong> How do you test data transformation logic?</summary>

**Answer:** I join source and target on the business key, then compare each column against the expected transformation. For example, if the mapping says to apply UPPER() to customer name, I write: WHERE target_name != UPPER(source_name). For CASE-based transformations like status code lookups, I verify every possible input value maps to the correct output. I also test edge cases — NULLs, empty strings, maximum length values.

</details>

<details>
<summary><strong>Q11:</strong> What is referential integrity and how do you check it?</summary>

**Answer:** Referential integrity means every foreign key in a child table has a matching primary key in the parent table. In a data warehouse, this means every account_id in fact_transaction should exist in dim_account. I check it using LEFT JOIN from fact to dimension, WHERE dimension_key IS NULL. Any results are "orphan records" — transactions pointing to non-existent accounts.

</details>

<details>
<summary><strong>Q12:</strong> What is the ETL testing lifecycle?</summary>

**Answer:** It follows six phases: Understand requirements and business rules. Review source-to-target mapping documents. Design test cases covering row counts, transformations, duplicates, integrity, and edge cases. Execute tests by running SQL queries against the data. Log defects with SQL evidence in JIRA. Retest fixes and perform regression testing before sign-off.

</details>

<details>
<summary><strong>Q13:</strong> What is data profiling?</summary>

**Answer:** Data profiling is analyzing source data to understand its structure, quality, and patterns before testing. It includes checking data distributions, NULL percentages, minimum/maximum values, distinct counts, and data type patterns. It helps me identify potential data quality issues early and design better test cases.

</details>

<details>
<summary><strong>Q14:</strong> How do you handle a situation where the ETL load succeeds but the data is wrong?</summary>

**Answer:** This is a functional failure — the ETL ran without technical errors but produced incorrect results. I first confirm the issue by running validation queries. Then I trace the data through each layer — source → staging → target — to find where the error was introduced. I document it with specific SQL evidence, raise a defect, and work with the developer to fix the transformation logic. This is why we can't just check ETL job status — we must validate the actual data.

</details>

<details>
<summary><strong>Q15:</strong> What is the difference between a full load and incremental load?</summary>

**Answer:** A full load extracts and loads ALL data from source to target every time — the target is typically truncated first. An incremental load only processes new or changed records since the last run, using timestamps, change flags, or CDC. Full loads are simpler but slower. Incremental loads are faster but require careful testing for duplicate inserts and missing updates.

</details>

---

## 💾 SQL Questions (20)

<details>
<summary><strong>Q16:</strong> What types of JOINs do you know? When would you use each?</summary>

**Answer:** INNER JOIN returns only matching rows — I use it when I need data that exists in both tables. LEFT JOIN returns all rows from the left table plus matches — this is my go-to for finding missing records. FULL OUTER JOIN shows mismatches in both directions. CROSS JOIN creates a Cartesian product — rarely needed. SELF JOIN joins a table to itself — used for hierarchical data like employee-manager relationships.

</details>

<details>
<summary><strong>Q17:</strong> How do you find records in Source A that are missing from Target B?</summary>

**Answer:** Two methods: First, LEFT JOIN: `SELECT a.* FROM source a LEFT JOIN target b ON a.key = b.key WHERE b.key IS NULL`. Second, EXCEPT: `SELECT key FROM source EXCEPT SELECT key FROM target`. LEFT JOIN is more flexible because I can see all source columns, not just the key.

</details>

<details>
<summary><strong>Q18:</strong> What is the difference between WHERE and HAVING?</summary>

**Answer:** WHERE filters individual rows before any aggregation happens. HAVING filters grouped results after aggregation. For example, WHERE amount > 100 excludes rows before counting. HAVING COUNT(*) > 5 filters groups after counting. You can't use aggregate functions in WHERE — that's a common mistake.

</details>

<details>
<summary><strong>Q19:</strong> How do you find duplicate records?</summary>

**Answer:** `SELECT key_column, COUNT(*) FROM table GROUP BY key_column HAVING COUNT(*) > 1`. This shows which keys have duplicates. To identify which specific rows are duplicates, I use ROW_NUMBER() OVER (PARTITION BY key ORDER BY load_date DESC) — any row with row_number > 1 is a duplicate.

</details>

<details>
<summary><strong>Q20:</strong> What is a window function? Name some examples.</summary>

**Answer:** Window functions perform calculations across rows related to the current row without collapsing the result set like GROUP BY. Key functions: ROW_NUMBER() for unique sequential numbering, RANK() for ranking with gaps, DENSE_RANK() for ranking without gaps, LAG() to look at the previous row, LEAD() to look at the next row, and SUM() OVER() for running totals.

</details>

<details>
<summary><strong>Q21:</strong> What does ROW_NUMBER, RANK, and DENSE_RANK return for tied values?</summary>

**Answer:** For values like 100, 200, 200, 300: ROW_NUMBER gives unique numbers — 1, 2, 3, 4 (no ties). RANK gives the same rank for ties then skips — 1, 2, 2, 4. DENSE_RANK gives the same rank for ties without skipping — 1, 2, 2, 3.

</details>

<details>
<summary><strong>Q22:</strong> How does NULL work in SQL comparisons?</summary>

**Answer:** NULL is not equal to anything — not even another NULL. `NULL = NULL` evaluates to NULL, not TRUE. Always use IS NULL or IS NOT NULL for checking. NULL in arithmetic produces NULL: `NULL + 100 = NULL`. COUNT(*) counts rows with NULLs, but COUNT(column) skips NULLs. Use COALESCE or ISNULL to replace NULLs with default values.

</details>

<details>
<summary><strong>Q23:</strong> What is a CTE and when would you use one?</summary>

**Answer:** A CTE (Common Table Expression) is a named temporary result set defined with the WITH clause. It makes complex queries more readable by breaking them into logical parts. For example, I can define a CTE to calculate customer totals, then JOIN it with a customer table in the main query. CTEs exist only for the duration of that single statement.

</details>

<details>
<summary><strong>Q24:</strong> What is the difference between UNION and UNION ALL?</summary>

**Answer:** UNION combines result sets and removes duplicates — it's slower because it has to sort and compare. UNION ALL combines result sets keeping all rows including duplicates — it's faster. I use UNION ALL for ETL validation when comparing source and target counts side by side, because I know there won't be duplicates across the two queries.

</details>

<details>
<summary><strong>Q25:</strong> Write a query to find the top 3 customers by total order amount.</summary>

**Answer:**
```sql
-- SQL Server syntax:
SELECT TOP 3 customer_id, SUM(amount) AS total_amount
FROM fact_orders
GROUP BY customer_id
ORDER BY total_amount DESC;
-- In MySQL, replace TOP 3 with LIMIT 3 at the end.
-- In Oracle, use FETCH FIRST 3 ROWS ONLY at the end (12c+).
```
Or using ROW_NUMBER (works on all three platforms):
```sql
WITH ranked AS (
    SELECT customer_id, SUM(amount) AS total,
           ROW_NUMBER() OVER (ORDER BY SUM(amount) DESC) AS rn
    FROM fact_orders GROUP BY customer_id
)
SELECT * FROM ranked WHERE rn <= 3;
```

</details>

<details>
<summary><strong>Q26:</strong> What does COALESCE do? Give an example.</summary>

**Answer:** COALESCE returns the first non-NULL value from a list. Example: `COALESCE(home_phone, work_phone, mobile, 'No Contact')` — it checks each value left to right and returns the first one that isn't NULL. If all are NULL, it returns 'No Contact'. It's essential in ETL for handling missing data.

</details>

<details>
<summary><strong>Q27:</strong> What is a correlated subquery?</summary>

**Answer:** A correlated subquery references columns from the outer query and executes once for each row of the outer query. Example: finding the latest order per customer: `SELECT * FROM orders o1 WHERE order_date = (SELECT MAX(order_date) FROM orders o2 WHERE o2.customer_id = o1.customer_id)`. It's slower than a regular subquery but sometimes necessary.

</details>

<details>
<summary><strong>Q28:</strong> How do you use CASE WHEN?</summary>

**Answer:** CASE WHEN is SQL's if-then-else expression. Example: `CASE WHEN balance >= 100000 THEN 'Platinum' WHEN balance >= 50000 THEN 'Gold' ELSE 'Standard' END`. In ETL, it's used for data categorization, code-to-description mapping, and implementing business rules during transformation.

</details>

<details>
<summary><strong>Q29:</strong> What is the difference between TRUNCATE and DELETE?</summary>

**Answer:** TRUNCATE is DDL — removes ALL rows, very fast, and resets identity columns. DELETE is DML — can target specific rows with WHERE, slower, doesn't reset identity. A key nuance: in SQL Server, TRUNCATE *can* be rolled back if it's inside an explicit transaction (`BEGIN TRAN ... TRUNCATE ... ROLLBACK`). In Oracle, TRUNCATE issues an implicit commit, so it truly cannot be rolled back. Know this difference — it shows depth. In ETL, TRUNCATE is used before full loads, DELETE for targeted cleanup.

</details>

<details>
<summary><strong>Q30:</strong> What is the difference between IN and EXISTS?</summary>

**Answer:** Both check if a value exists in a result set. IN is simpler: `WHERE id IN (SELECT id FROM table2)`. EXISTS is more efficient for large datasets because it stops scanning as soon as it finds a match: `WHERE EXISTS (SELECT 1 FROM table2 WHERE table2.id = table1.id)`. For small datasets, IN is fine. For large ones, EXISTS is better.

</details>

<details>
<summary><strong>Q31:</strong> How do you create a running total in SQL?</summary>

**Answer:** Using a window function with SUM: `SUM(amount) OVER (PARTITION BY account_id ORDER BY txn_date)`. This gives a cumulative sum of amounts for each account ordered by date. In banking ETL, this is used to calculate running account balances.

</details>

<details>
<summary><strong>Q32:</strong> What is a temp table and when would you use one?</summary>

**Answer:** A temp table is a temporary storage that persists for the duration of a session. Created with `SELECT ... INTO #temp_table` or `CREATE TABLE #temp`. I use them in ETL testing when I need to store intermediate validation results and refer to them multiple times. They're faster than running the same complex query repeatedly.

</details>

<details>
<summary><strong>Q33:</strong> What are indexes and how do they affect performance?</summary>

**Answer:** An index is like a book's index — it helps SQL find data faster without scanning every row. Clustered indexes physically sort the table data (one per table). Non-clustered indexes are separate structures (many per table). Indexes speed up SELECT/WHERE but slow down INSERT/UPDATE/DELETE because the index must be updated too.

</details>

<details>
<summary><strong>Q34:</strong> Write a query to compare row counts between source and target.</summary>

**Answer:**
```sql
SELECT 'Source' AS system, COUNT(*) AS row_count FROM source_db.customers
UNION ALL
SELECT 'Target' AS system, COUNT(*) AS row_count FROM target_db.dim_customer;
```
This gives a side-by-side comparison. If the counts differ, I'd drill down using LEFT JOIN to find specifics.

</details>

<details>
<summary><strong>Q35:</strong> How do you handle date format differences between systems?</summary>

**Answer:** I verify the source date format first, then check if the ETL correctly converts it. A common issue is MM/DD vs DD/MM confusion — March 1 (03/01) gets interpreted as January 3 (01/03). I compare source and target dates using CAST and CONVERT, and flag any mismatches. Using ISO format YYYY-MM-DD avoids ambiguity.

</details>

---

## 🧪 Testing Methodology Questions (10)

<details>
<summary><strong>Q36:</strong> What is regression testing in the context of ETL?</summary>

**Answer:** Regression testing means re-running existing test cases after a change or fix to ensure nothing previously working is broken. In ETL, this means after a transformation logic fix, I revalidate not just the fixed data flow but also related data flows that might be affected. It catches unintended side effects of changes.

</details>

<details>
<summary><strong>Q37:</strong> How do you prioritize which tests to run first?</summary>

**Answer:** I prioritize by risk and impact: Row counts first (catches missing/extra records quickly), then critical business fields (amounts, dates, keys), then referential integrity (orphan records), then transformations (business rule accuracy), then edge cases. Financial amounts in banking always get top priority because errors there have the most severe business impact.

</details>

<details>
<summary><strong>Q38:</strong> How do you document an ETL defect?</summary>

**Answer:** A well-documented defect includes: a clear title ("500 orphan records in FACT_TRANSACTION — missing accounts in DIM_ACCOUNT"), the environment and date, source and target counts, the exact SQL query I used to identify the issue, sample data (5-10 affected records), severity assessment, and expected vs actual behavior. I log these in JIRA with all the SQL evidence.

</details>

<details>
<summary><strong>Q39:</strong> What is the difference between positive and negative testing?</summary>

**Answer:** Positive testing verifies that valid data is processed correctly — records appear in the target, transformations are accurate, lookups work. Negative testing verifies that invalid data is handled properly — invalid records are rejected or quarantined, error messages are logged, no partial records exist in target tables. Both are essential in ETL.

</details>

<details>
<summary><strong>Q40:</strong> What is boundary testing in ETL?</summary>

**Answer:** Boundary testing checks edge cases: maximum length strings (does a 200-character name truncate?), zero and negative amounts, dates at year boundaries (Dec 31 → Jan 1), NULL values, empty strings, special characters, and Unicode. These are where ETL bugs most commonly hide because developers often code for the "happy path" and miss edge cases.

</details>

<details>
<summary><strong>Q41:</strong> How do you test an ETL in Agile?</summary>

**Answer:** In Agile, ETL testing is integrated into sprints. I participate in sprint planning to understand scope, review user stories involving data changes, design test cases during the sprint, execute tests as soon as ETL code is ready, and provide feedback quickly. I attend daily standups to report test progress and blockers. Defects are raised in the current sprint's JIRA board.

</details>

<details>
<summary><strong>Q42:</strong> What tools have you used for ETL testing?</summary>

**Answer:** My primary tool is SQL — I write queries directly against source and target databases for validation. For test management, I've used JIRA for defect tracking and qTest for test case management. I've worked with SQL Server Management Studio and Oracle SQL Developer as database clients. For reporting, I've used Excel for data comparison and documentation.

</details>

<details>
<summary><strong>Q43:</strong> How do you ensure test coverage for ETL?</summary>

**Answer:** I map test cases directly to the source-to-target mapping document. Each mapping rule gets at least one test case. I also add test cases for: row counts (one per table), duplicate checks (one per unique key), referential integrity (one per FK relationship), and edge cases (at least 3 per transformation). I use a traceability matrix to ensure 100% mapping coverage.

</details>

<details>
<summary><strong>Q44:</strong> How do you handle a tight deadline when not all tests can be completed?</summary>

**Answer:** I prioritize by risk: critical data flows and financial amounts first, then core business rules, then lower-risk validations. I communicate clearly with the team about what's covered and what's deferred. For deferred items, I document them and ensure they're picked up in the next sprint. Creating reusable SQL templates also saves time across similar tables.

</details>

<details>
<summary><strong>Q45:</strong> What metrics do you track for ETL testing?</summary>

**Answer:** Key metrics include: test case pass/fail rate, defect count by severity, row count accuracy (% match between source and target), data reconciliation variances, test coverage percentage against the mapping document, and average defect resolution time. These help me report overall data quality health to stakeholders.

</details>

---

## 💼 Behavioral / Situational Questions (5)

<details>
<summary><strong>Q46:</strong> Tell me about yourself and your experience.</summary>

**Answer:** I'm a QA Engineer with over 6 years of experience, focused on ETL testing and data validation in banking and retail domains. I started in ETL testing, writing SQL queries for source-to-target validation, data accuracy, and completeness checks. I progressed to leading QA for modules, designing test strategies, and managing defect lifecycles. I've also worked in business analysis, which gave me deeper understanding of how data serves business needs. I'm looking to leverage my strong SQL and data validation skills in this role.

</details>

<details>
<summary><strong>Q47:</strong> Describe a challenging data issue you found and how you resolved it.</summary>

**Answer:** On a banking project, I found a $100 mismatch in daily transaction totals between source and target. I drilled down by category using aggregate comparison queries and found the issue was in the "Transfer" category. Further investigation revealed a data type issue — the source used DECIMAL(10,2) but the ETL had a FLOAT intermediate step, introducing tiny rounding errors. Across thousands of transactions, these micro-differences added up to $100. I raised it as a critical defect, and the fix was to use DECIMAL throughout the pipeline.

</details>

<details>
<summary><strong>Q48:</strong> How do you communicate with developers about defects?</summary>

**Answer:** I focus on clarity and evidence. I provide the exact SQL query, the actual vs expected results, sample data, and the business impact. I avoid blame — instead of "the developer coded it wrong," I say "the transformation for column X doesn't match the mapping specification." I also discuss severity objectively — financial amounts get Critical, formatting issues get Minor. This approach has always led to productive collaboration.

</details>

<details>
<summary><strong>Q49:</strong> Why are you interested in this ETL/SQL role?</summary>

**Answer:** My career started in ETL testing, and that's where I've always been most engaged — working hands-on with data and SQL. While my recent roles broadened my perspective — especially understanding how data serves business reporting — I want to return to what I do best: ensuring data quality through rigorous testing. I bring a unique combination of technical SQL skills and business context understanding, which helps me ask "is this data correct?" not just "does the query run?"

</details>

<details>
<summary><strong>Q50:</strong> Where do you see yourself in 2-3 years?</summary>

**Answer:** I see myself as a senior ETL tester or data quality lead, deep in complex data validation challenges. I want to develop expertise in automation of ETL testing — creating reusable validation frameworks. I'm also interested in expanding into data engineering concepts to understand the full data pipeline end-to-end. In the shorter term, I want to master this role and become the go-to person for data quality on the team.

</details>

---

> 💡 **Practice Tip:** Don't just read these answers. **Say them out loud.** Record yourself if possible. The goal is to sound natural and confident, not memorized.

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">🎤 50 questions mastered = interview confidence achieved! 💪</p>
