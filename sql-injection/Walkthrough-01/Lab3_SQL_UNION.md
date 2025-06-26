# 📄 SQL Injection UNION Attacks – Simplified Guide

- A SQL Injection UNION Attack is a technique used by attackers to extract data from a database by injecting malicious SQL code. This is possible when a web application is vulnerable to SQL injection and includes the results of SQL queries in its response.

### Q. What Is the UNION Keyword?
- In SQL, the UNION keyword is used to combine the results of two or more SELECT queries into a single result set.

Example:
```
SELECT a, b FROM table1
UNION
SELECT c, d FROM table2
```

- This query returns results from both table1 and table2, assuming both queries return the same number of columns with compatible data types.

## Key Requirements for `UNION Attacks`

### To successfully use the UNION keyword in a SQL injection attack, two conditions must be met:

- Same number of columns in both the original and injected queries.
- Compatible data types in corresponding columns (e.g., numbers matched with numbers, strings with strings).

#### Step 1: Finding the Number of Columns
- You need to know how many columns the original SQL query returns. Here are two common methods:

  - #### Method 1: Using `ORDER BY`
    Inject ORDER BY clauses with increasing column numbers until an error occurs.

```
' ORDER BY 1-- 
' ORDER BY 2-- 
' ORDER BY 3--
```
- When the column number in ORDER BY exceeds the actual number of columns, the database will return an error like:

   1. The ORDER BY position number 3 is out of range.. 
  
   2. This tells you how many columns are in the result.

  - #### Method 2: Using UNION SELECT NULL
         Inject multiple NULL values using the UNION SELECT command, increasing the count until the query works.

```
' UNION SELECT NULL-- 
' UNION SELECT NULL, NULL-- 
' UNION SELECT NULL, NULL, NULL--
```


### If the number of NULLs doesn't match the number of original columns, an error will occur:

- All queries combined using a UNION must have the same number of expressions.
- Once the number matches, the database returns a valid result, possibly showing extra output (like an extra row).

#### Why use NULL?
- Because NULL is compatible with all data types, making it safe to use for testing.

#### Step 2: Matching Data Types
- Once you know the number of columns, you need to identify which columns can display your data (like text or numbers). This helps you know where to inject meaningful values later (e.g., usernames, passwords).

## Important Notes
- Only works if the application returns query results in the HTTP response.

- Errors may not be shown directly; observe changes in the page output to detect behavior.

- This method is commonly used in labs or penetration testing — do not use it without permission.

---

# 🧪 Lab: SQL Injection UNION Attack – Determining the Number of Columns Returned by the Query

## 🔍 Objective
The goal of this lab is to determine the number of columns returned by the original SQL query using a UNION-based SQL injection vulnerability.

### 🛠️ Lab Setup and Initial Analysis
- Launching the Lab Environment

![Website](/sql-injection/Screenshots/1.1.1sql.png)

- After starting the lab, the Burp Suite browser was launched to ensure a clean testing environment with no third-party interference.
- Navigating the Target Application
- Within the lab's web application, the "Gifts" section was accessed. This section contains a search bar parameter, which appears to be vulnerable and is the primary point of interest for injection testing.

![Website](/sql-injection/Screenshots/1.1.2sql.png)

#### Capturing the Request
- A search query was submitted via the application's search bar. This request was then captured using Burp Suite's HTTP proxy.

![Website](/sql-injection/Screenshots/1.1.3sql.png)

#### Sending the Request to Repeater
- For controlled and repeatable testing, the captured request was sent to Burp Suite's Repeater tab, where SQL payloads could be manually injected and analyzed.

## 🧪 Manual Testing via the Browser (Search Bar)
Before performing the injection through Burp Suite, manual testing was conducted directly via the browser's search functionality.

#### ➤ Testing with ORDER BY Clause
The ORDER BY clause was used to determine the number of columns in the original query. The following payloads were injected one by one into the search parameter:
```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
' ORDER BY 4--
```
![Website](/sql-injection/Screenshots/1.1.4sql.png)

![Website](/sql-injection/Screenshots/1.1.5sql.png)

![Website](/sql-injection/Screenshots/1.1.6sql.png)

#### 🧾 Observed Behavior:
- `' ORDER BY 1,' ORDER BY 2, and ' ORDER BY 3` returned normal results, indicating that these column positions exist.

#### - `' ORDER BY 4` caused an `Internal Server Error`, signaling that there is no fourth column in the result set.

![Website](/sql-injection/Screenshots/1.1.7sql.png)


### ✅ Conclusion:
From this test, we can infer that the original SQL query returns exactly 3 columns.

---

## 🧩 Phase 2: Using UNION SELECT to Confirm Column Count
After determining through the ORDER BY testing that the query returns 3 columns, the next step is to confirm this using the UNION SELECT method.

##### 🔄 Using Burp Suite Repeater for Injection

Modifying the Request:

- In Burp Suite's Repeater, the previously captured search request is modified to test for a UNION-based SQL injection.

- The parameter category=Gifts is modified to inject SQL directly.

We remove the value 'Gifts' and instead inject a payload such as:

```
' UNION SELECT NULL--
```

![Website](/sql-injection/Screenshots/1.1.8sql.png)

#### Testing Column Counts:

- We gradually increase the number of NULL values in the UNION SELECT statement until no error is returned:

```
' UNION SELECT NULL--           → Error
' UNION SELECT NULL,NULL--      → Error
' UNION SELECT NULL,NULL,NULL-- → ✅ Success
' UNION SELECT NULL,NULL,NULL,NULL-- → Error
```

![Website](/sql-injection/Screenshots/1.1.9sql.png)

![Website](/sql-injection/Screenshots/1.1.10sql.png)

![Website](/sql-injection/Screenshots/1.1.11sql.png)



#### Observing Responses:

- For 1 and 2 columns: The response contains an internal server error or empty result, indicating the number of columns is incorrect.

- For 3 columns: The server returns a valid response without an error.

- For 4 columns: Again, an error is observed.

#### ✅ Conclusion

- Through successful payload testing using UNION SELECT, we confirmed that:

- The underlying SQL query expects 3 columns.

- When we send UNION SELECT NULL,NULL,NULL--, the application accepts the request, confirming our guess.

- At this point, the lab detects success as we have correctly determined and matched the number of columns, completing the objective.


