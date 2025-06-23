The website is using this query in the backend when you choose a product category like "Gifts":

```
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```
This means: `Show only products` where: The category is  `Gifts` AND the released flag is 1 `(which means it's public/visible)`

- Your goal:
-- 👉 Trick this query to also show unreleased products (those with released = 0), using SQL Injection.

# 🧪 Step-by-Step Explanation

## 🔍 Step 1: Find where to inject

On the website, you might see a URL like this when you click a product category:
```
https://example.com/filter?category=Gifts
```
![Website](/sql-injection/Screenshots/1.sql.png)

- This means the website is likely running a query like:

```
sql: SELECT * FROM products WHERE category = 'Gifts' AND released = 1;
```

![Website](/sql-injection/Screenshots/2.sql.png)


## 🧨 Step 2: Inject the Payload

- Now, try replacing Gifts in the URL with this input:

```
1' OR 1=1--
```
- What are we doing here?

  - `1'`  This closes the original input value in the SQL query.

  - `OR 1=1 ` This is a logical condition that is always true. It says: “If the first part fails, just show everything where 1 equals 1 (which is always true).”

  - `--` This is a SQL comment operator. It tells the database to ignore the rest of the query that might cause errors or prevent the injection from working (like AND released = 1).

![Website](/sql-injection/Screenshots/3.sql.png)

- Execue the payload.
  
![Website](/sql-injection/Screenshots/4.sql.png)

- We can see all the data i.e. the Gifts present which shows that it is vulnerable to sql injection.
  
![Website](/sql-injection/Screenshots/5.sql.png)

---

### BOOM !! LAB SOLVED !!......
