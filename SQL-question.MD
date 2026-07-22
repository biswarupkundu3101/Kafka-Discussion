# SQL Interview Questions for 5 Years Experience Developer
## Focus: Performance Optimization, Window Functions, Functions, Indexes & Cursors

---

## 1. **Window Functions & Performance**

1. **Write a query using ROW_NUMBER() to get the top 3 performing employees in each department. How would you optimize this for a table with 10 million records?**
   - Expected Answer: Should discuss partitioning, indexing on (department_id, salary), and avoiding OFFSET/FETCH in nested queries.

2. **How would you use RANK() vs DENSE_RANK() vs ROW_NUMBER() to identify duplicate sales in the last 30 days? What performance implications exist?**
   - Expected Answer: Discussion on when each is appropriate, CTE usage, and composite indexes.

3. **Write a window function query that calculates running totals for each customer. How would you prevent performance degradation for customers with 100K+ transactions?**
   - Expected Answer: Should cover frame specifications (ROWS BETWEEN), partition sizing, and index strategies.

4. **How do you optimize a query using LAG() and LEAD() functions over a large dataset? What indexes would you create?**
   - Expected Answer: Discussion on covering indexes, proper ORDER BY clauses within PARTITION BY, and avoiding unnecessary lookups.

5. **Write a performance-optimized query using PERCENT_RANK() to find sales in top 10% of each region. How would you measure its execution time?**
   - Expected Answer: Should discuss execution plans, statistics, and potential parallelization.

---

## 2. **Indexes & Query Optimization**

6. **Design an indexing strategy for a query that uses window functions with PARTITION BY on multiple columns. What would be your approach?**
   - Expected Answer: Composite indexes, covering indexes, index hints, and query plan analysis.

7. **Explain the difference between a clustered and non-clustered index. How would you choose between them for a table that heavily uses window functions?**
   - Expected Answer: Storage implications, query patterns, and performance trade-offs.

8. **Write a query to find missing indexes that could improve performance. What columns would you prioritize for indexing?**
   - Expected Answer: System queries (sys.dm_missing_indexes), filtering criteria, and ROI analysis.

9. **How do you handle index fragmentation for tables that are frequently updated? What's the performance impact?**
   - Expected Answer: Fragmentation thresholds, REBUILD vs REORGANIZE, maintenance schedules.

10. **Design a covering index for a query that uses window functions with ORDER BY on multiple columns. How would you test its effectiveness?**
    - Expected Answer: Index key columns, included columns, statistics, and query plan comparison.

---

## 3. **Functions & Performance**

11. **Write a scalar user-defined function (UDF) to calculate employee tenure and discuss its performance implications when used in large queries.**
    - Expected Answer: Should mention that scalar UDFs can cause row-by-row execution; better to use inline TVF or window functions.

12. **How would you convert a scalar function into a more performant inline table-valued function (iTVF)? Show an example.**
    - Expected Answer: Comparison of execution plans, SARGability, and batch processing.

13. **Write a deterministic function that caches results. How would you implement this for maximum performance?**
    - Expected Answer: SCHEMABINDING, computed columns, indexed views, statistics.

14. **Discuss the performance difference between using a function in WHERE clause vs in SELECT clause with a large table.**
    - Expected Answer: SARGability issues, index usage, and query plan examples.

15. **How do you optimize date functions like DATEDIFF() and DATEADD() for performance in queries filtering millions of records?**
    - Expected Answer: Pre-calculated columns, index on computed columns, and avoiding function calls in WHERE clause.

---

## 4. **Cursors & Performance Concerns**

16. **When is it appropriate to use a cursor? Write a cursor-based solution and explain why it's generally a performance anti-pattern.**
    - Expected Answer: Rare cases (CLR integration, complex procedural logic), set-based alternatives.

17. **Convert a CURSOR-based stored procedure into a set-based solution. How much performance improvement would you expect?**
    - Expected Answer: 10-100x improvement typical; demonstrate with both implementations.

18. **Write a cursor that processes batch updates and explain how to achieve the same result with a set-based approach that's faster.**
    - Expected Answer: Loop vs bulk operations, minimizing locks, transaction scope.

19. **How do you monitor cursor performance? What metrics would you track?**
    - Expected Answer: Cursor properties, memory usage, locking behavior, execution time.

20. **Discuss STATIC vs DYNAMIC vs KEYSET cursors. Which is most performant and why?**
    - Expected Answer: Memory, locking, runtime behavior, and trade-offs.

---

## 5. **Complex Performance Scenarios**

21. **Write a query using multiple window functions that calculates rank, running total, and percentage within partitions. How would you optimize this for 50M+ records?**
    - Expected Answer: Single pass processing, proper indexing, statistics, and parallelization.

22. **How do you identify a query that uses a non-sargable expression? Provide examples and optimization techniques.**
    - Expected Answer: Function calls, implicit conversions, OR conditions, and rewrites.

23. **Design a solution for calculating year-over-year metrics using window functions. How would you ensure sub-second performance?**
    - Expected Answer: Partitioning strategy, pre-aggregation, indexed views, materialized data.

24. **Write a recursive CTE with window functions to build an organizational hierarchy. How would you prevent performance issues with deep hierarchies?**
    - Expected Answer: MAXRECURSION, anchor members, indexing on parent_id, and iteration limits.

25. **Create an index recommendation system. How would you identify the top 5 indexes that would most improve query performance across the entire database?**
    - Expected Answer: Missing index DMVs, query cost analysis, and impact assessment.

---

## 6. **Execution Plans & Analysis**

26. **Interpret an execution plan showing a Table Scan with high I/O cost. What's your remediation strategy?**
    - Expected Answer: Missing indexes, data type mismatches, statistics updates, and rewrite strategies.

27. **How do you identify index seek vs index scan? Write a query that forces a seek and one that causes a scan.**
    - Expected Answer: Query rewrites, predicate pushdown, and statistics impact.

28. **Analyze a query plan showing key lookup operations. How would you eliminate them?**
    - Expected Answer: Covering indexes, included columns, and trade-off analysis.

29. **What's the difference between a nested loop join, hash join, and sort merge join? When would each be optimal?**
    - Expected Answer: Data distribution, memory, I/O patterns, and query hints.

30. **How do you detect and resolve parameter sniffing issues in stored procedures?**
    - Expected Answer: Local variables, RECOMPILE hints, plan guides, and parameter value analysis.

---

## 7. **Window Functions - Advanced Scenarios**

31. **Write a window function query that identifies consecutive days of sales for each customer. How would you handle data gaps efficiently?**
    - Expected Answer: ROW_NUMBER() with DATEDIFF trick, ranking logic, and large data handling.

32. **Design a query using NTILE() to segment customers into quartiles based on lifetime value. Optimize for real-time calculation.**
    - Expected Answer: Incremental statistics, batch processing, and index strategy.

33. **Create a leaderboard query using window functions that updates real-time. How would you ensure sub-100ms response time?**
    - Expected Answer: Caching, pre-aggregation, materialized views, and indexed queries.

34. **Write a complex query using FIRST_VALUE() and LAST_VALUE() with frame specifications. Discuss performance considerations.**
    - Expected Answer: Frame definitions impact, proper indexing, and UNBOUNDED clauses.

35. **How would you optimize a query calculating moving averages over 365 days of data using window functions?**
    - Expected Answer: Window frame size optimization, statistics, partitioning strategy.

---

## 8. **Index Design & Composite Keys**

36. **Design a composite index for a query filtering by region, then product category, then sales amount. Explain the SARGability impact.**
    - Expected Answer: Column order matters, leading columns, statistics, and query plan impact.

37. **When would you use a filtered index? Provide an example and discuss performance benefits.**
    - Expected Answer: Storage savings, maintenance reduction, and use case scenarios.

38. **How do you choose between a wide index vs multiple narrow indexes for the same query?**
    - Expected Answer: Memory, maintenance, query patterns, and trade-offs.

39. **Write a query to analyze index usage statistics. What metrics would indicate a poorly performing index?**
    - Expected Answer: sys.dm_db_index_usage_stats, seeks vs scans, fragmentation, and unused indexes.

40. **Design an indexing strategy for a slowly changing dimension (SCD) table. What challenges exist?**
    - Expected Answer: Temporal queries, effective date ranges, and query optimization.

---

## 9. **Aggregate Functions & Window Functions**

41. **Compare GROUP BY vs window functions for calculating aggregates. When is each appropriate? Performance implications?**
    - Expected Answer: Result structure, memory usage, sorting requirements, and parallelization.

42. **Write a query combining window functions with GROUPING SETS. How would you optimize this complex aggregation?**
    - Expected Answer: Proper indexing, materialized views, and incremental computation.

43. **Design a cumulative performance dashboard query that must execute in <500ms for 100M+ transactions.**
    - Expected Answer: Pre-aggregation, indexed views, time-series optimization, and batching.

44. **How do you handle NULL values in window functions? Performance implications of various approaches?**
    - Expected Answer: NULL handling clauses, index design, and alternative solutions.

45. **Write a query using window aggregate functions to calculate percentiles. How would you ensure accuracy with large datasets?**
    - Expected Answer: PERCENTILE_CONT vs PERCENTILE_DISC, appropriate indexing, and statistical accuracy.

---

## 10. **Real-World Performance Challenges**

46. **You inherit a stored procedure that joins 8 tables and uses cursors. It takes 5 minutes to execute. How would you optimize it step-by-step?**
    - Expected Answer: Profiling approach, cursor elimination, index analysis, execution plan review, and incremental testing.

47. **A report query runs in 2 seconds but needs to run in <200ms. Design your optimization approach.**
    - Expected Answer: Caching strategy, materialized views, denormalization, and sampling techniques.

48. **How would you implement a high-performance search function using full-text search? Index design and query optimization?**
    - Expected Answer: FTS index creation, catalog setup, and query construction best practices.

49. **Design a solution for efficiently calculating running/cumulative totals across 500M+ financial transactions.**
    - Expected Answer: Partitioning, incremental computation, index strategy, and verification approach.

50. **Your query performs well in development (10K rows) but slow in production (100M rows). What's your diagnostic and resolution process?**
    - Expected Answer: Statistics update, execution plan comparison, data distribution analysis, and index re-evaluation.

---

## Answer Evaluation Criteria

**Excellent Answer Should Include:**
- Specific SQL syntax and examples
- Execution plan considerations
- Index design rationale
- Performance metrics or benchmarking approach
- Trade-offs and alternative solutions
- Scalability discussion for large datasets
- Real-world implementation experience
- Monitoring and maintenance considerations

**Red Flags:**
- Ignoring performance implications
- Not considering indexes in solutions
- Over-reliance on cursors or WHILE loops
- Suggesting complex solutions when simpler ones exist
- Not discussing statistics and query plans
- Missing validation of optimization results

---

## Bonus: Performance Troubleshooting Checklist

When optimizing SQL queries, follow this systematic approach:

1. **Establish Baseline** - Measure current execution time
2. **Analyze Execution Plan** - Look for table scans, key lookups, and inefficient joins
3. **Check Statistics** - Ensure statistics are up-to-date
4. **Review Indexes** - Identify missing or fragmented indexes
5. **Evaluate Query Logic** - Rewrite for better efficiency
6. **Test Incrementally** - One change at a time
7. **Monitor Production** - Ensure optimization holds under real load
8. **Document Changes** - Keep improvement notes for future reference

