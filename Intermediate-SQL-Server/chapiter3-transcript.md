1. WHILE loops
   Processing data using loops is a common technique in many programming languages. In this lesson, we are going to review modifying the data by creating variables and using them in a WHILE loop.

2. Using variables in T-SQL
   Creating variables in your T-SQL queries are useful in several instances. In T-SQL, you create variables using the DECLARE keyword. The first character of the variable name must start with the at-sign and you must assign a valid data type to the variable.

3. Variable data types in T-SQL
   T-SQL supports several different kinds of data types. The most common ones are VARCHAR, INT, and DECIMAL. VARCHAR can be used to store strings of variable length where n is the string length in bytes. INT can be used to store positive and negative integers in the range of 2 billion. DECIMAL can be used to store real numbers. p is the total number of digits that will be stored on either side of the decimal. The argument, s, specifies the number of digits to be stored on the right of the decimal. DECIMAL and NUMERIC are synonymous and work the exact same way, so you can use one or the other to store numbers with decimals.

4. Declaring variables in T-SQL
   As mentioned earlier, you need to use the DECLARE keyword to declare variables in T-SQL. Here, we declare the variable Snack and assign the data type VARCHAR with length 10. Note that the variable name begins with the at-sign.

5. Assigning values to variables
   There are two different methods for assigning a value to a variable. You can either use the reserved keyword SET or use SELECT to assign a value to a variable. When you assign a value to a variable using SET or SELECT there is no result shown. To see the value of a variable you need to write a separate SELECT statement to return the value as shown here.

6. WHILE loops
   While loops in T-SQL work the same way as in any other programming language. They need to start with the keyword WHILE and some kind of condition, such as x less than 5, which will be evaluated every time the loop is run. The next keyword is BEGIN, which is used to denote the beginning of the loop. The code you want to run starts here and continues until the END keyword. If you want to break out of the loop in the middle, use the BREAK keyword. If you want to continue the loop, then use the keyword CONTINUE.

7. WHILE loop in T-SQL (I)
   Here we see the standard syntax for a while statement. First, we declare and set the variable ctr to 1. We then specify the WHILE keyword with the condition ctr less than 10. This condition tells us that the WHILE loop will run as long as the value of ctr is less than 10. After this, we write the code to be executed between the BEGIN and END keywords. We increment the value of ctr by 1 using the SET keyword. As soon as the value of ctr reaches 10 inside the loop, the loop stops because the while statement condition ctr must be less than 10, evaluates to false. After the loop ends, we print the value in the variable ctr.

8. WHILE loop in T-SQL (II)
   In this example, the BREAK command exits out of the loop prior to the loop ending. If you want to exit the WHILE loop when ctr is equal to 4, you can write a conditional check inside the WHILE loop. The part where you are incrementing the counter is the same as before. But now instead of ending the loop, you write an IF statement to check if ctr is equal to 4 and use the BREAK keyword to exit the loop if that it is true. This code will only run 3 times and then exit from the loop. If you print the value of ctr at the end of this loop, you will notice that it is equal to 4.

9. Let's practice!
   Now it's your turn to practice WHILE loops in T-SQL.
