# 📝 Cheat Sheet — Quick Reference

<div class="section-banner">
  <span class="banner-icon">⚡</span>
  <span class="banner-text">Your one-page reference. Review this the night before and morning of the interview. Covers all critical SQL syntax, ETL concepts, and must-know terminology.</span>
</div>

---

<div class="cheat-grid">

<div class="cheat-section">
  <div class="cheat-title">💾 SQL Query Structure</div>

| Clause | Purpose | Order |
|--------|---------|-------|
| `SELECT` | Choose columns | 1st in query |
| `FROM` | Specify tables | 2nd |
| `JOIN` | Combine tables | 3rd |
| `WHERE` | Filter rows (before agg) | 4th |
| `GROUP BY` | Group rows for aggregation | 5th |
| `HAVING` | Filter groups (after agg) | 6th |
| `ORDER BY` | Sort results | 7th |
| `LIMIT/TOP` | Restrict rows returned | 8th |

</div>

<div class="cheat-section">
  <div class="cheat-title">🔗 JOIN Types</div>

| JOIN | Returns |
|------|---------|
| `INNER` | Only matching rows |
| `LEFT` | All left + matching right |
| `RIGHT` | All right + matching left |
| `FULL OUTER` | All from both tables |
| `CROSS` | Cartesian product |
| `SELF` | Table joined to itself |

</div>

</div>

<div class="cheat-grid">

<div class="cheat-section">
  <div class="cheat-title">📊 Aggregate Functions</div>

| Function | Description |
|----------|-------------|
| `COUNT(*)` | Count all rows (incl NULLs) |
| `COUNT(col)` | Count non-NULL values |
| `COUNT(DISTINCT col)` | Count unique values |
| `SUM(col)` | Total of numeric values |
| `AVG(col)` | Average value |
| `MIN(col)` | Smallest value |
| `MAX(col)` | Largest value |

</div>

<div class="cheat-section">
  <div class="cheat-title">🪟 Window Functions</div>

| Function | Description |
|----------|-------------|
| `ROW_NUMBER()` | Unique sequential (no ties) |
| `RANK()` | Same for ties, skips next |
| `DENSE_RANK()` | Same for ties, no skip |
| `LAG(col, n)` | Value from n rows before |
| `LEAD(col, n)` | Value from n rows after |
| `SUM() OVER()` | Running total |

</div>

</div>

<div class="cheat-grid">

<div class="cheat-section">
  <div class="cheat-title">🚫 NULL Handling</div>

| Expression | Use For |
|-----------|---------|
| `IS NULL` | Check if NULL |
| `IS NOT NULL` | Check if not NULL |
| `COALESCE(a,b,c)` | First non-NULL value |
| `ISNULL(a, default)` | Replace NULL (SQL Server) |
| `NULLIF(a, b)` | Return NULL if a = b |

**Gotchas:** NULL = NULL → NULL (not TRUE!) · NULL + 100 → NULL · COUNT(*) includes NULLs, COUNT(col) doesn't

</div>

<div class="cheat-section">
  <div class="cheat-title">🔤 String Functions</div>

| Function | Example |
|----------|---------|
| `CONCAT(a,b)` | Join strings |
| `UPPER(s)` | To uppercase |
| `LOWER(s)` | To lowercase |
| `TRIM(s)` | Remove spaces |
| `SUBSTRING(s,start,len)` | Extract part |
| `REPLACE(s,old,new)` | Replace text |
| `LEN(s)` / `LENGTH(s)` | String length |

</div>

</div>

---

## 🧪 ETL Testing Checklist

| # | Check | SQL Pattern |
|---|-------|-------------|
| 1 | **Row Count** | `SELECT COUNT(*) FROM source` vs target |
| 2 | **Duplicates** | `GROUP BY key HAVING COUNT(*) > 1` |
| 3 | **Missing Records** | `LEFT JOIN ... WHERE target_key IS NULL` |
| 4 | **NULLs** | `WHERE column IS NULL` (mandatory fields) |
| 5 | **Transformations** | `WHERE target_col != EXPECTED(source_col)` |
| 6 | **Referential Integrity** | `LEFT JOIN fact→dim WHERE dim_key IS NULL` |
| 7 | **Aggregates** | `SUM(source) = SUM(target)` |
| 8 | **Data Types** | Check target column types match mapping |
| 9 | **Boundaries** | Max lengths, future dates, negative amounts |
| 10 | **Incremental** | New records only, no re-inserts |

---

## 📖 Key Terminology

<div class="cheat-grid">

<div class="cheat-section">
  <div class="cheat-title">🔄 ETL Terms</div>

| Term | Definition |
|------|-----------|
| **Staging Area** | Temp zone for raw source data |
| **Source-to-Target Mapping** | Column-level transformation spec |
| **Data Lineage** | Tracing data origin → target |
| **Data Profiling** | Analyzing data quality patterns |
| **Full Load** | Extract & load ALL data |
| **Incremental Load** | Only new/changed records |
| **Batch Window** | Scheduled time for ETL execution |
| **CDC** | Change Data Capture |

</div>

<div class="cheat-section">
  <div class="cheat-title">🏛️ Data Warehouse Terms</div>

| Term | Definition |
|------|-----------|
| **Fact Table** | Measurable events (transactions) |
| **Dimension Table** | Descriptive attributes (customers) |
| **Surrogate Key** | System-generated ID (no meaning) |
| **Natural Key** | Business key from source data |
| **SCD Type 1** | Overwrite (no history) |
| **SCD Type 2** | New row with dates (keeps history) |
| **Star Schema** | Facts in center, dims around |
| **Data Mart** | Subset of DW for one department |

</div>

</div>

---

## 🎯 Interview Day Checklist

- [ ] Review this cheat sheet one more time
- [ ] Have your self-introduction ready (2 minutes)
- [ ] Know your resume talking points cold
- [ ] Prepare 2-3 STAR answers
- [ ] Have 3-5 questions to ask the interviewer
- [ ] Keep a pen and paper ready for drawing/writing
- [ ] Test your internet/audio/video if virtual
- [ ] Dress professionally
- [ ] Take a deep breath — you've prepared well! 💪

---

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">📝 Print this page or keep it open on a second screen for quick reference!</p>
