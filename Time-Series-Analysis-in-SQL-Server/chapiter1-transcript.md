1. Building dates
   Welcome! In this course, we will look at a number of techniques for working with time series data using SQL Server.

2. This guy
   My name is Kevin Feasel. I am a Microsoft Data Platform MVP and am a co-founder at Envizage, a company specializing in data analysis and visualization.

3. What you will learn
   Throughout this course, we will use built-in functionality to solve a number of business problems with SQL Server. To start, we will learn how SQL Server handles the individual parts which make up a date and translating strings--for example, from CSV files--to dates. We will then take advantage of these functions to control the grain of our dates. This includes, for example, downsampling data to an hourly grain for analysis. We will then cover specific business cases around windows of data, including running totals, moving averages, and where date ranges overlap.

4. Building a date
   In this first lesson, we will review some of the basics around date handling in SQL Server. You should already be familiar with the DATETIME and DATETIME2 data types. Let's review a few functions which use these date types to make your life easier. First, GETDATE() and GETUTCDATE() will return a date, in local or UTC time respectively, as a DATETIME data type. Similarly, SYSDATETIME() and SYSUTCDATETIME() return the current time--either local or UTC--as a DATETIME2 typed response. In both cases, we get the dates and times we expect.

5. Breaking down a date
   Suppose we don't need an entire date and time but only want a specific part. For a date like this one, there are a few built-in functions. We can get the year. Or the month. Or the day. This might not be enough to satisfy us, so we need to take it to the next level.

6. Parsing dates with date parts
   The DATEPART() and DATENAME() functions give us much more control over our date parts. DATEPART() returns to us the numeric value of the part we want, such as the year. By contrast, DATENAME() gives us a string value, important if we want to know the name of the month. These functions can shred a date into a number of component parts. We can get the year, month, and day like the standalone functions. We can also get the day of the year or day of the week. We can get the week of the year or the ISO week of the year--a format which is used mostly in Europe. We can even go small, getting minutes, seconds, milliseconds or even down to nanoseconds!

7. Adding and subtracting dates
   Parsing out date parts is not the only useful thing we can do with T-SQL. We can also add and subtract dates with the DATEADD() function. For example, suppose we have a day. We can add days to move forward in time or subtract days to move backward in time. We can even chain these function calls together for additional precision.

8. Comparing dates
   The DATEDIFF() function allows us to compare the number of units of time between two date or time types. For example, suppose we have a start and end time for a machine. We can easily determine the number of seconds, minutes, or hours elapsed. Be warned that DATEDIFF() returns an integer and rounds up.

9. Let's practice!
   Now that we've seen some of the basics of handling date parts, let's jump into a few exercises.
