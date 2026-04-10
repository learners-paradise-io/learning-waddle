# 🏆 Timed Quiz

<div class="section-banner">
  <span class="banner-icon">⏱️</span>
  <span class="banner-text">25 questions. 12 minutes. Test yourself under real interview pressure. Click an answer to see if you're right!</span>
</div>

---

<div class="quiz-header" id="quizHeader">
  <div class="quiz-timer" id="quizTimer">⏱️ <span id="timerDisplay">12:00</span></div>
  <div class="quiz-score" id="quizScore">Score: 0 / 25</div>
  <div class="quiz-progress"><div class="quiz-progress-bar" id="quizProgressBar" style="width:0%"></div></div>
</div>

<div id="quizContainer">

  <div class="quiz-question" id="q1">
    <div class="quiz-q-num">Question 1 of 25</div>
    <div class="quiz-q-text">What does ETL stand for?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(1,this,'a','a')"><span class="opt-letter">A</span> Extract, Transform, Load</div>
      <div class="quiz-opt" onclick="answer(1,this,'b','a')"><span class="opt-letter">B</span> Extract, Transfer, Load</div>
      <div class="quiz-opt" onclick="answer(1,this,'c','a')"><span class="opt-letter">C</span> Export, Transform, Load</div>
      <div class="quiz-opt" onclick="answer(1,this,'d','a')"><span class="opt-letter">D</span> Extract, Transform, Link</div>
    </div>
    <div class="quiz-explain">ETL = <strong>Extract</strong> data from sources, <strong>Transform</strong> (clean/validate/reshape), <strong>Load</strong> into target system.</div>
  </div>

  <div class="quiz-question" id="q2">
    <div class="quiz-q-num">Question 2 of 25</div>
    <div class="quiz-q-text">Which JOIN would you use to find records in the source that are missing from the target?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(2,this,'a','b')"><span class="opt-letter">A</span> INNER JOIN</div>
      <div class="quiz-opt" onclick="answer(2,this,'b','b')"><span class="opt-letter">B</span> LEFT JOIN with WHERE target_key IS NULL</div>
      <div class="quiz-opt" onclick="answer(2,this,'c','b')"><span class="opt-letter">C</span> CROSS JOIN</div>
      <div class="quiz-opt" onclick="answer(2,this,'d','b')"><span class="opt-letter">D</span> RIGHT JOIN</div>
    </div>
    <div class="quiz-explain">LEFT JOIN returns all source records + matching target records. Adding <code>WHERE target_key IS NULL</code> filters to only the missing ones.</div>
  </div>

  <div class="quiz-question" id="q3">
    <div class="quiz-q-num">Question 3 of 25</div>
    <div class="quiz-q-text">What is the result of `NULL = NULL` in SQL?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(3,this,'a','c')"><span class="opt-letter">A</span> TRUE</div>
      <div class="quiz-opt" onclick="answer(3,this,'b','c')"><span class="opt-letter">B</span> FALSE</div>
      <div class="quiz-opt" onclick="answer(3,this,'c','c')"><span class="opt-letter">C</span> NULL (unknown)</div>
      <div class="quiz-opt" onclick="answer(3,this,'d','c')"><span class="opt-letter">D</span> Error</div>
    </div>
    <div class="quiz-explain">NULL = NULL evaluates to <strong>NULL</strong> (unknown), not TRUE. Always use <code>IS NULL</code> to check for NULLs.</div>
  </div>

  <div class="quiz-question" id="q4">
    <div class="quiz-q-num">Question 4 of 25</div>
    <div class="quiz-q-text">What is the difference between WHERE and HAVING?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(4,this,'a','a')"><span class="opt-letter">A</span> WHERE filters before aggregation; HAVING filters after</div>
      <div class="quiz-opt" onclick="answer(4,this,'b','a')"><span class="opt-letter">B</span> HAVING filters before aggregation; WHERE filters after</div>
      <div class="quiz-opt" onclick="answer(4,this,'c','a')"><span class="opt-letter">C</span> They are interchangeable</div>
      <div class="quiz-opt" onclick="answer(4,this,'d','a')"><span class="opt-letter">D</span> WHERE is for SELECT; HAVING is for UPDATE</div>
    </div>
    <div class="quiz-explain"><strong>WHERE</strong> filters individual rows before GROUP BY. <strong>HAVING</strong> filters grouped results after aggregation.</div>
  </div>

  <div class="quiz-question" id="q5">
    <div class="quiz-q-num">Question 5 of 25</div>
    <div class="quiz-q-text">How do you find duplicate records in a table?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(5,this,'a','b')"><span class="opt-letter">A</span> SELECT DISTINCT * FROM table</div>
      <div class="quiz-opt" onclick="answer(5,this,'b','b')"><span class="opt-letter">B</span> SELECT col, COUNT(*) GROUP BY col HAVING COUNT(*) > 1</div>
      <div class="quiz-opt" onclick="answer(5,this,'c','b')"><span class="opt-letter">C</span> SELECT * WHERE duplicate = true</div>
      <div class="quiz-opt" onclick="answer(5,this,'d','b')"><span class="opt-letter">D</span> SELECT TOP 2 FROM table</div>
    </div>
    <div class="quiz-explain">GROUP BY the key column + <code>HAVING COUNT(*) > 1</code> shows which values appear more than once.</div>
  </div>

  <div class="quiz-question" id="q6">
    <div class="quiz-q-num">Question 6 of 25</div>
    <div class="quiz-q-text">What does COALESCE(phone, mobile, 'N/A') return?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(6,this,'a','a')"><span class="opt-letter">A</span> The first non-NULL value from phone, mobile, or 'N/A'</div>
      <div class="quiz-opt" onclick="answer(6,this,'b','a')"><span class="opt-letter">B</span> Always returns 'N/A'</div>
      <div class="quiz-opt" onclick="answer(6,this,'c','a')"><span class="opt-letter">C</span> Concatenates all three values</div>
      <div class="quiz-opt" onclick="answer(6,this,'d','a')"><span class="opt-letter">D</span> Returns NULL if any value is NULL</div>
    </div>
    <div class="quiz-explain">COALESCE returns the <strong>first non-NULL value</strong>. If phone is NULL but mobile has a value, it returns mobile.</div>
  </div>

  <div class="quiz-question" id="q7">
    <div class="quiz-q-num">Question 7 of 25</div>
    <div class="quiz-q-text">In ETL testing, what does "data reconciliation" mean?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(7,this,'a','c')"><span class="opt-letter">A</span> Deleting duplicate records</div>
      <div class="quiz-opt" onclick="answer(7,this,'b','c')"><span class="opt-letter">B</span> Transforming data formats</div>
      <div class="quiz-opt" onclick="answer(7,this,'c','c')"><span class="opt-letter">C</span> Comparing summary totals (counts, sums) between source and target</div>
      <div class="quiz-opt" onclick="answer(7,this,'d','c')"><span class="opt-letter">D</span> Creating backup copies of data</div>
    </div>
    <div class="quiz-explain">Data reconciliation compares <strong>aggregate values</strong> (row counts, SUM of amounts) between source and target to verify completeness.</div>
  </div>

  <div class="quiz-question" id="q8">
    <div class="quiz-q-num">Question 8 of 25</div>
    <div class="quiz-q-text">What does ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY load_date DESC) do?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(8,this,'a','b')"><span class="opt-letter">A</span> Counts total rows per customer</div>
      <div class="quiz-opt" onclick="answer(8,this,'b','b')"><span class="opt-letter">B</span> Assigns a unique number to each row within each customer, latest first</div>
      <div class="quiz-opt" onclick="answer(8,this,'c','b')"><span class="opt-letter">C</span> Deletes duplicate rows</div>
      <div class="quiz-opt" onclick="answer(8,this,'d','b')"><span class="opt-letter">D</span> Groups rows by customer</div>
    </div>
    <div class="quiz-explain">ROW_NUMBER assigns a <strong>unique sequential number</strong> within each partition. Row 1 = most recent record for each customer.</div>
  </div>

  <div class="quiz-question" id="q9">
    <div class="quiz-q-num">Question 9 of 25</div>
    <div class="quiz-q-text">What is an incremental load?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(9,this,'a','b')"><span class="opt-letter">A</span> Loading all data from scratch every time</div>
      <div class="quiz-opt" onclick="answer(9,this,'b','b')"><span class="opt-letter">B</span> Loading only new or changed records since the last run</div>
      <div class="quiz-opt" onclick="answer(9,this,'c','b')"><span class="opt-letter">C</span> Loading data in random order</div>
      <div class="quiz-opt" onclick="answer(9,this,'d','b')"><span class="opt-letter">D</span> Loading data one table at a time</div>
    </div>
    <div class="quiz-explain">Incremental load processes only <strong>new or changed</strong> records, using timestamps or change flags. Full load reloads everything.</div>
  </div>

  <div class="quiz-question" id="q10">
    <div class="quiz-q-num">Question 10 of 25</div>
    <div class="quiz-q-text">What is the difference between UNION and UNION ALL?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(10,this,'a','c')"><span class="opt-letter">A</span> UNION is faster</div>
      <div class="quiz-opt" onclick="answer(10,this,'b','c')"><span class="opt-letter">B</span> UNION ALL removes duplicates</div>
      <div class="quiz-opt" onclick="answer(10,this,'c','c')"><span class="opt-letter">C</span> UNION removes duplicates; UNION ALL keeps them</div>
      <div class="quiz-opt" onclick="answer(10,this,'d','c')"><span class="opt-letter">D</span> They are the same</div>
    </div>
    <div class="quiz-explain"><strong>UNION</strong> removes duplicates (slower). <strong>UNION ALL</strong> keeps all rows (faster).</div>
  </div>

  <div class="quiz-question" id="q11">
    <div class="quiz-q-num">Question 11 of 25</div>
    <div class="quiz-q-text">What is a surrogate key?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(11,this,'a','c')"><span class="opt-letter">A</span> A key from the source business data</div>
      <div class="quiz-opt" onclick="answer(11,this,'b','c')"><span class="opt-letter">B</span> A foreign key in a fact table</div>
      <div class="quiz-opt" onclick="answer(11,this,'c','c')"><span class="opt-letter">C</span> A system-generated ID with no business meaning</div>
      <div class="quiz-opt" onclick="answer(11,this,'d','c')"><span class="opt-letter">D</span> An encrypted version of the primary key</div>
    </div>
    <div class="quiz-explain">Surrogate keys are <strong>system-generated</strong> (like sequence numbers). They have no business meaning and are preferred in data warehouses.</div>
  </div>

  <div class="quiz-question" id="q12">
    <div class="quiz-q-num">Question 12 of 25</div>
    <div class="quiz-q-text">What does SCD Type 2 do?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(12,this,'a','b')"><span class="opt-letter">A</span> Overwrites old data with new data</div>
      <div class="quiz-opt" onclick="answer(12,this,'b','b')"><span class="opt-letter">B</span> Keeps history with start/end dates and a current flag</div>
      <div class="quiz-opt" onclick="answer(12,this,'c','b')"><span class="opt-letter">C</span> Deletes old records</div>
      <div class="quiz-opt" onclick="answer(12,this,'d','b')"><span class="opt-letter">D</span> Stores changes in a separate audit table</div>
    </div>
    <div class="quiz-explain">SCD Type 2 <strong>preserves history</strong> by creating new rows with effective dates and a current/active flag.</div>
  </div>

  <div class="quiz-question" id="q13">
    <div class="quiz-q-num">Question 13 of 25</div>
    <div class="quiz-q-text">Which function gets the value from the previous row?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(13,this,'a','a')"><span class="opt-letter">A</span> LAG()</div>
      <div class="quiz-opt" onclick="answer(13,this,'b','a')"><span class="opt-letter">B</span> LEAD()</div>
      <div class="quiz-opt" onclick="answer(13,this,'c','a')"><span class="opt-letter">C</span> PREVIOUS()</div>
      <div class="quiz-opt" onclick="answer(13,this,'d','a')"><span class="opt-letter">D</span> PRIOR()</div>
    </div>
    <div class="quiz-explain"><strong>LAG()</strong> gets a value from a previous row. <strong>LEAD()</strong> gets a value from the next row.</div>
  </div>

  <div class="quiz-question" id="q14">
    <div class="quiz-q-num">Question 14 of 25</div>
    <div class="quiz-q-text">What does COUNT(*) count compared to COUNT(column)?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(14,this,'a','b')"><span class="opt-letter">A</span> They always return the same result</div>
      <div class="quiz-opt" onclick="answer(14,this,'b','b')"><span class="opt-letter">B</span> COUNT(*) counts all rows; COUNT(column) counts only non-NULL values</div>
      <div class="quiz-opt" onclick="answer(14,this,'c','b')"><span class="opt-letter">C</span> COUNT(*) is faster</div>
      <div class="quiz-opt" onclick="answer(14,this,'d','b')"><span class="opt-letter">D</span> COUNT(column) counts all rows; COUNT(*) counts non-NULL values</div>
    </div>
    <div class="quiz-explain"><code>COUNT(*)</code> counts <strong>all rows including NULLs</strong>. <code>COUNT(column)</code> counts only <strong>non-NULL</strong> values.</div>
  </div>

  <div class="quiz-question" id="q15">
    <div class="quiz-q-num">Question 15 of 25</div>
    <div class="quiz-q-text">What is the most common cause of missing records in ETL?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(15,this,'a','a')"><span class="opt-letter">A</span> NULL values in JOIN keys causing records to be dropped</div>
      <div class="quiz-opt" onclick="answer(15,this,'b','a')"><span class="opt-letter">B</span> Slow network connections</div>
      <div class="quiz-opt" onclick="answer(15,this,'c','a')"><span class="opt-letter">C</span> Database server running out of memory</div>
      <div class="quiz-opt" onclick="answer(15,this,'d','a')"><span class="opt-letter">D</span> Incorrect SQL syntax</div>
    </div>
    <div class="quiz-explain">NULL values in JOIN keys are silently dropped by <strong>INNER JOINs</strong> — the #1 cause of missing records in ETL.</div>
  </div>

  <div class="quiz-question" id="q16">
    <div class="quiz-q-num">Question 16 of 25</div>
    <div class="quiz-q-text">What is a CTE (Common Table Expression)?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(16,this,'a','c')"><span class="opt-letter">A</span> A permanent table in the database</div>
      <div class="quiz-opt" onclick="answer(16,this,'b','c')"><span class="opt-letter">B</span> A stored procedure</div>
      <div class="quiz-opt" onclick="answer(16,this,'c','c')"><span class="opt-letter">C</span> A named temporary result set defined with WITH, existing for one query</div>
      <div class="quiz-opt" onclick="answer(16,this,'d','c')"><span class="opt-letter">D</span> A type of index</div>
    </div>
    <div class="quiz-explain">A CTE is defined with <code>WITH name AS (query)</code> — it makes complex queries more readable and only exists for that single statement.</div>
  </div>

  <div class="quiz-question" id="q17">
    <div class="quiz-q-num">Question 17 of 25</div>
    <div class="quiz-q-text">What is a fact table in a data warehouse?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(17,this,'a','b')"><span class="opt-letter">A</span> A table storing descriptive attributes</div>
      <div class="quiz-opt" onclick="answer(17,this,'b','b')"><span class="opt-letter">B</span> A table storing measurable events/transactions with FK to dimensions</div>
      <div class="quiz-opt" onclick="answer(17,this,'c','b')"><span class="opt-letter">C</span> A temp table used during ETL</div>
      <div class="quiz-opt" onclick="answer(17,this,'d','b')"><span class="opt-letter">D</span> A lookup table</div>
    </div>
    <div class="quiz-explain">Fact tables store <strong>measurable events</strong> (transactions, orders) and have foreign keys pointing to dimension tables.</div>
  </div>

  <div class="quiz-question" id="q18">
    <div class="quiz-q-num">Question 18 of 25</div>
    <div class="quiz-q-text">What is the difference between TRUNCATE and DELETE?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(18,this,'a','d')"><span class="opt-letter">A</span> TRUNCATE is slower than DELETE</div>
      <div class="quiz-opt" onclick="answer(18,this,'b','d')"><span class="opt-letter">B</span> DELETE can't use WHERE clause</div>
      <div class="quiz-opt" onclick="answer(18,this,'c','d')"><span class="opt-letter">C</span> They are the same operation</div>
      <div class="quiz-opt" onclick="answer(18,this,'d','d')"><span class="opt-letter">D</span> TRUNCATE removes all rows fast (no rollback); DELETE can target rows (can rollback)</div>
    </div>
    <div class="quiz-explain">TRUNCATE is a DDL command — fast, removes all rows, can't rollback. DELETE is DML — slower, can filter with WHERE, can rollback.</div>
  </div>

  <div class="quiz-question" id="q19">
    <div class="quiz-q-num">Question 19 of 25</div>
    <div class="quiz-q-text">What does the EXCEPT operator do?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(19,this,'a','a')"><span class="opt-letter">A</span> Returns rows from the first query that don't exist in the second</div>
      <div class="quiz-opt" onclick="answer(19,this,'b','a')"><span class="opt-letter">B</span> Returns all rows from both queries</div>
      <div class="quiz-opt" onclick="answer(19,this,'c','a')"><span class="opt-letter">C</span> Returns only rows that exist in both queries</div>
      <div class="quiz-opt" onclick="answer(19,this,'d','a')"><span class="opt-letter">D</span> Throws an error if queries don't match</div>
    </div>
    <div class="quiz-explain">EXCEPT returns rows in the <strong>first query that are NOT in the second</strong>. Great for finding missing records between source and target.</div>
  </div>

  <div class="quiz-question" id="q20">
    <div class="quiz-q-num">Question 20 of 25</div>
    <div class="quiz-q-text">In RANK() window function, what happens with tied values?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(20,this,'a','b')"><span class="opt-letter">A</span> Ties get different ranks</div>
      <div class="quiz-opt" onclick="answer(20,this,'b','b')"><span class="opt-letter">B</span> Ties get the same rank, then it skips numbers (1,2,2,4)</div>
      <div class="quiz-opt" onclick="answer(20,this,'c','b')"><span class="opt-letter">C</span> Ties get the same rank, no skip (1,2,2,3)</div>
      <div class="quiz-opt" onclick="answer(20,this,'d','b')"><span class="opt-letter">D</span> Ties cause an error</div>
    </div>
    <div class="quiz-explain">RANK gives ties the <strong>same number then skips</strong> (1,2,2,4). DENSE_RANK gives ties the same number <strong>without skipping</strong> (1,2,2,3).</div>
  </div>

  <div class="quiz-question" id="q21">
    <div class="quiz-q-num">Question 21 of 25</div>
    <div class="quiz-q-text">What is a source-to-target mapping document?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(21,this,'a','c')"><span class="opt-letter">A</span> A list of all tables in the database</div>
      <div class="quiz-opt" onclick="answer(21,this,'b','c')"><span class="opt-letter">B</span> A test execution report</div>
      <div class="quiz-opt" onclick="answer(21,this,'c','c')"><span class="opt-letter">C</span> A document defining how source columns map to target columns with transformations</div>
      <div class="quiz-opt" onclick="answer(21,this,'d','c')"><span class="opt-letter">D</span> A database schema diagram</div>
    </div>
    <div class="quiz-explain">The mapping doc is the ETL tester's primary reference — it defines source → transformation → target for every column.</div>
  </div>

  <div class="quiz-question" id="q22">
    <div class="quiz-q-num">Question 22 of 25</div>
    <div class="quiz-q-text">What is referential integrity in the context of a data warehouse?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(22,this,'a','b')"><span class="opt-letter">A</span> All tables have the same number of rows</div>
      <div class="quiz-opt" onclick="answer(22,this,'b','b')"><span class="opt-letter">B</span> Every foreign key in a fact table has a matching primary key in a dimension table</div>
      <div class="quiz-opt" onclick="answer(22,this,'c','b')"><span class="opt-letter">C</span> All columns are NOT NULL</div>
      <div class="quiz-opt" onclick="answer(22,this,'d','b')"><span class="opt-letter">D</span> Data is encrypted</div>
    </div>
    <div class="quiz-explain">Referential integrity ensures every FK (e.g., account_id in transactions) has a matching PK in the referenced table (dim_account).</div>
  </div>

  <div class="quiz-question" id="q23">
    <div class="quiz-q-num">Question 23 of 25</div>
    <div class="quiz-q-text">What is the staging area in ETL?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(23,this,'a','a')"><span class="opt-letter">A</span> A temporary holding area where raw source data lands before transformation</div>
      <div class="quiz-opt" onclick="answer(23,this,'b','a')"><span class="opt-letter">B</span> The final destination for data</div>
      <div class="quiz-opt" onclick="answer(23,this,'c','a')"><span class="opt-letter">C</span> A backup database</div>
      <div class="quiz-opt" onclick="answer(23,this,'d','a')"><span class="opt-letter">D</span> A testing environment</div>
    </div>
    <div class="quiz-explain">The staging area is a <strong>temporary holding zone</strong> between source and target. Raw data lands here before transformations are applied.</div>
  </div>

  <div class="quiz-question" id="q24">
    <div class="quiz-q-num">Question 24 of 25</div>
    <div class="quiz-q-text">What is the key difference between ETL and ELT?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(24,this,'a','c')"><span class="opt-letter">A</span> ETL is newer than ELT</div>
      <div class="quiz-opt" onclick="answer(24,this,'b','c')"><span class="opt-letter">B</span> ELT doesn't transform data</div>
      <div class="quiz-opt" onclick="answer(24,this,'c','c')"><span class="opt-letter">C</span> ETL transforms before loading; ELT loads first, then transforms in the target</div>
      <div class="quiz-opt" onclick="answer(24,this,'d','c')"><span class="opt-letter">D</span> ETL uses SQL; ELT uses Python</div>
    </div>
    <div class="quiz-explain">ETL transforms in a staging area before loading. ELT loads raw data into the target first, then transforms using the target's compute power.</div>
  </div>

  <div class="quiz-question" id="q25">
    <div class="quiz-q-num">Question 25 of 25</div>
    <div class="quiz-q-text">What does NULLIF(status, 'Unknown') return?</div>
    <div class="quiz-options">
      <div class="quiz-opt" onclick="answer(25,this,'a','a')"><span class="opt-letter">A</span> NULL if status equals 'Unknown', otherwise the status value</div>
      <div class="quiz-opt" onclick="answer(25,this,'b','a')"><span class="opt-letter">B</span> 'Unknown' if status is NULL</div>
      <div class="quiz-opt" onclick="answer(25,this,'c','a')"><span class="opt-letter">C</span> Always returns NULL</div>
      <div class="quiz-opt" onclick="answer(25,this,'d','a')"><span class="opt-letter">D</span> Always returns 'Unknown'</div>
    </div>
    <div class="quiz-explain">NULLIF returns <strong>NULL if the two arguments are equal</strong>. If status = 'Unknown', it returns NULL. Otherwise, it returns the status value.</div>
  </div>

</div>

<div class="quiz-results" id="quizResults">
  <div style="font-size: 2rem;">🏆</div>
  <div class="result-score" id="resultScore">0/25</div>
  <div class="result-grade" id="resultGrade"></div>
  <div class="result-msg" id="resultMsg"></div>
</div>

<p style="text-align: center; color: #94a3b8; margin-top: 2rem; font-size: 0.85rem;">⏱️ Beat the clock and aim for 20/25 or higher!</p>
