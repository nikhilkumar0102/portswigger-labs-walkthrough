📄 SQL Injection UNION Attacks – Simplified Guide
Overview
A SQL Injection UNION Attack is a technique used by attackers to extract data from a database by injecting malicious SQL code. This is possible when a web application is vulnerable to SQL injection and includes the results of SQL queries in its response.

What Is the UNION Keyword?
In SQL, the UNION keyword is used to combine the results of two or more SELECT queries into a single result set.

Example:
sql
Copy
Edit
SELECT a, b FROM table1
UNION
SELECT c, d FROM table2
This query returns results from both table1 and table2, assuming both queries return the same number of columns with compatible data types.

Key Requirements for UNION Attacks
To successfully use the UNION keyword in a SQL injection attack, two conditions must be met:

Same number of columns in both the original and injected queries.

Compatible data types in corresponding columns (e.g., numbers matched with numbers, strings with strings).

Step 1: Finding the Number of Columns
You need to know how many columns the original SQL query returns. Here are two common methods:

Method 1: Using ORDER BY
Inject ORDER BY clauses with increasing column numbers until an error occurs.

sql
Copy
Edit
' ORDER BY 1-- 
' ORDER BY 2-- 
' ORDER BY 3-- 
When the column number in ORDER BY exceeds the actual number of columns, the database will return an error like:

The ORDER BY position number 3 is out of range.

This tells you how many columns are in the result.

Method 2: Using UNION SELECT NULL
Inject multiple NULL values using the UNION SELECT command, increasing the count until the query works.

sql
Copy
Edit
' UNION SELECT NULL-- 
' UNION SELECT NULL, NULL-- 
' UNION SELECT NULL, NULL, NULL-- 
If the number of NULLs doesn't match the number of original columns, an error will occur:

All queries combined using a UNION must have the same number of expressions.

Once the number matches, the database returns a valid result, possibly showing extra output (like an extra row).

Why use NULL?
Because NULL is compatible with all data types, making it safe to use for testing.

Step 2: Matching Data Types
Once you know the number of columns, you need to identify which columns can display your data (like text or numbers). This helps you know where to inject meaningful values later (e.g., usernames, passwords).

Important Notes
Only works if the application returns query results in the HTTP response.

Errors may not be shown directly; observe changes in the page output to detect behavior.

This method is commonly used in labs or penetration testing — do not use it without permission.
