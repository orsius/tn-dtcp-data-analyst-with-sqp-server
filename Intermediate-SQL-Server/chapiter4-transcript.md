1. Window functions
   In this lesson, we will introduce windowing functions, which provide the ability to create and analyze groups of data. With windowing functions, you can look at the current row, the next row, and the previous row all at the same time very efficiently.

2. Applying windows to data in a table
   Using windowing functions, data within a table is processed as a group, allowing each group to be evaluated separately. In the example shown here, the data is broken into windows based upon the SalesYear column.

3. Grouping data in T-SQL
   This query shows how you can return all of the data for 2011. Using windowing functions you can create a query to return values by year without knowing what the values for the year are.

4. Window syntax in T-SQL
   A window is created by using the OVER clause. Next, inside the parentheses, you may provide the optional reserved keywords PARTITION BY to create the window boundary based on the specified columns. If you do not include PARTITION BY, the frame is created for the entire table. To arrange the output, you can use ORDER BY. After you create the window, you can add new functions.

5. Window functions (SUM)
   Here we partition the table by SalesYear and use the windowing function SUM to add up every row of the CurrentQuota column in the window to provide a total for the entire window in the YearTotal column. When the year changes, the value in the YearTotal column changes showing the total for the next year because the window is being partitioned by SalesYear. We are not showing all of the rows for 2012 here, just one row for 2012.

6. Window functions (COUNT)
   In addition to using SUM, you can also use other aggregations in windowing functions. Here we created the same window as the last slide, but are using COUNT instead of SUM. Just like for SUM, the value in the column QuotaPerYear, starts over when the year changes.

7. Let's practice!
   It's time for you to practice windowing functions!
