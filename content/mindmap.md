# 🧠 Mind Map & Visual Aids

<div class="section-banner">
  <span class="banner-icon">🗺️</span>
  <span class="banner-text">Visual diagrams, flow charts, and memory tricks to help you internalize key concepts. A picture is worth a thousand words — especially during an interview!</span>
</div>

---

## ETL Architecture — The Big Picture

```mermaid
flowchart TB
    subgraph Sources ["🏢 Source Systems"]
        S1["Oracle CRM"]
        S2["Flat Files (CSV)"]
        S3["REST APIs"]
        S4["Excel Sheets"]
    end

    subgraph ETL ["⚙️ ETL Pipeline"]
        E["📥 EXTRACT<br/>Pull raw data"]
        ST["📋 STAGING<br/>Raw data landing zone"]
        T["🔄 TRANSFORM<br/>Clean · Validate · Reshape"]
    end

    subgraph Target ["📊 Target Systems"]
        DW["🏛️ Data Warehouse"]
        DM["📈 Data Marts"]
        R["📊 Reports & Dashboards"]
    end

    S1 --> E
    S2 --> E
    S3 --> E
    S4 --> E
    E --> ST
    ST --> T
    T --> DW
    DW --> DM
    DM --> R
```

---

## SQL JOIN Types — Visual Decision Guide

```mermaid
flowchart TD
    Q["Which JOIN do I need?"]
    Q --> M{"Do I need ALL rows<br/>from both tables?"}
    M -->|Yes| FO["FULL OUTER JOIN<br/>Shows mismatches both ways"]
    M -->|No| L{"Do I need ALL rows<br/>from the LEFT table?"}
    L -->|Yes| LJ["LEFT JOIN<br/>+ WHERE right IS NULL<br/>= Find missing records"]
    L -->|No| O{"Only matching rows?"}
    O -->|Yes| IJ["INNER JOIN<br/>Only rows with matches"]
    O -->|No| CJ["CROSS JOIN<br/>Every possible combination"]
```

---

## ETL Testing Types — Hierarchy

```mermaid
flowchart TB
    A["🧪 ETL Testing"]
    A --> B["Row Count<br/>Validation"]
    A --> C["Data Completeness"]
    A --> D["Data Accuracy"]
    A --> E["Referential<br/>Integrity"]
    A --> F["Duplicate<br/>Check"]
    A --> G["Transformation<br/>Validation"]
    A --> H["Incremental<br/>Load Testing"]

    B --> B1["Source count<br/>= Target count?"]
    C --> C1["No NULLs in<br/>mandatory columns?"]
    D --> D1["Values correct<br/>after transformation?"]
    E --> E1["Every FK has<br/>a matching PK?"]
    F --> F1["No unintended<br/>duplicates?"]
    G --> G1["Business rules<br/>applied correctly?"]
    H --> H1["Only new/changed<br/>records loaded?"]
```

---

## Data Validation Decision Tree

*"I found a row count mismatch — now what?"*

```mermaid
flowchart TD
    START["⚠️ Row Count Mismatch<br/>Source ≠ Target"]
    START --> DIR{"Target has MORE<br/>or FEWER records?"}

    DIR -->|FEWER records| F["Missing Records"]
    F --> F1["Run LEFT JOIN:<br/>source LEFT JOIN target<br/>WHERE target_key IS NULL"]
    F1 --> F2{"Pattern in<br/>missing records?"}
    F2 -->|All have NULL keys| F3["🐛 NULL JOIN keys<br/>dropped by INNER JOIN"]
    F2 -->|All same status| F4["🐛 Filter too<br/>restrictive"]
    F2 -->|Random| F5["🐛 Constraint violations<br/>Check error logs"]

    DIR -->|MORE records| M["Duplicate Records"]
    M --> M1["Run GROUP BY + HAVING:<br/>GROUP BY key<br/>HAVING COUNT > 1"]
    M1 --> M2{"Same load dates?"}
    M2 -->|Yes| M3["🐛 Incremental load<br/>no duplicate check"]
    M2 -->|No| M4["🐛 Multiple source<br/>systems or SCD issue"]
```

---

## Window Functions Comparison — Quick Reference

<div class="flow-row">
  <div class="flow-node highlight">Data: 100, 200, 200, 300</div>
</div>

| Function | Result for 200 (first) | Result for 200 (second) | Result for 300 |
|----------|----------------------|------------------------|----------------|
| **ROW_NUMBER()** | 2 | 3 | 4 |
| **RANK()** | 2 | 2 | **4** (skips 3) |
| **DENSE_RANK()** | 2 | 2 | **3** (no skip) |

> 🧠 **Memory Trick:** "RANK **skips**, DENSE_RANK **doesn't skip**, ROW_NUMBER **never ties**"

---

## Memory Tricks & Mnemonics

<div class="mnemonic-grid">
  <div class="mnemonic-card">
    <div class="mnemonic-title">🧠 ETL Validation Order</div>
    <div class="mnemonic-phrase">"Count, Dupe, Transform, Reference"</div>
    <div class="mnemonic-explain"><strong>C</strong>ount rows → <strong>D</strong>up check → <strong>T</strong>ransformation verify → <strong>R</strong>eferential integrity. This is the order to validate after every ETL load.</div>
  </div>
  <div class="mnemonic-card">
    <div class="mnemonic-title">🧠 NULL Rules</div>
    <div class="mnemonic-phrase">"NULL is NOT equal to ANYTHING"</div>
    <div class="mnemonic-explain">NULL = NULL → NULL (not TRUE!). NULL + 100 → NULL. Always use IS NULL, never = NULL. COUNT(*) counts NULLs, COUNT(col) doesn't.</div>
  </div>
  <div class="mnemonic-card">
    <div class="mnemonic-title">🧠 WHERE vs HAVING</div>
    <div class="mnemonic-phrase">"WHERE Before, HAVING After"</div>
    <div class="mnemonic-explain">WHERE filters individual rows <strong>before</strong> GROUP BY aggregation. HAVING filters grouped results <strong>after</strong> aggregation. Can't use SUM() in WHERE.</div>
  </div>
  <div class="mnemonic-card">
    <div class="mnemonic-title">🧠 LEFT JOIN for Missing</div>
    <div class="mnemonic-phrase">"LEFT + IS NULL = What's Missing"</div>
    <div class="mnemonic-explain">LEFT JOIN source to target, WHERE target_key IS NULL → shows records in source but NOT in target. The #1 query for ETL validation.</div>
  </div>
  <div class="mnemonic-card">
    <div class="mnemonic-title">🧠 SCD Types</div>
    <div class="mnemonic-phrase">"Type 1 = Overwrite, Type 2 = History"</div>
    <div class="mnemonic-explain">SCD1 = update in place, no history. SCD2 = new row with dates + current flag, keeps full history. Banking needs Type 2 for audit.</div>
  </div>
  <div class="mnemonic-card">
    <div class="mnemonic-title">🧠 Fact vs Dimension</div>
    <div class="mnemonic-phrase">"Facts = Verbs, Dimensions = Nouns"</div>
    <div class="mnemonic-explain">Facts store events (transactions, sales = verbs). Dimensions store descriptions (customers, products = nouns). Facts have FKs to dimensions.</div>
  </div>
</div>

---

## ETL Testing Lifecycle — Step by Step

<div class="flow-row" style="flex-wrap: wrap; justify-content: center;">
  <div class="flow-node info">📋 Requirements</div>
  <span class="flow-arrow">→</span>
  <div class="flow-node info">📄 Mapping Docs</div>
  <span class="flow-arrow">→</span>
  <div class="flow-node highlight">✏️ Test Cases</div>
  <span class="flow-arrow">→</span>
  <div class="flow-node highlight">🔄 Execute</div>
  <span class="flow-arrow">→</span>
  <div class="flow-node warning">🐛 Defects</div>
  <span class="flow-arrow">→</span>
  <div class="flow-node success">✅ Sign-off</div>
</div>

---

## Key Differences — Quick Visual

| Feature | ETL | ELT |
|---------|-----|-----|
| **Transform where?** | Staging area | Target database |
| **Best for?** | Traditional DW | Cloud (Snowflake, BigQuery) |
| **Speed** | Slower for big data | Uses target's compute power |

| Feature | TRUNCATE | DELETE |
|---------|----------|--------|
| **Scope** | All rows | Can use WHERE |
| **Speed** | Very fast | Slower |
| **Rollback?** | ❌ No | ✅ Yes |
| **Type** | DDL | DML |

| Feature | Surrogate Key | Natural Key |
|---------|--------------|-------------|
| **Source** | System-generated | Business data |
| **Meaning** | None | Has business meaning |
| **Preferred in DW?** | ✅ Yes | ❌ Can change/reuse |

---

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">🧠 Review these visuals before the interview — they'll help you explain concepts clearly and quickly!</p>
