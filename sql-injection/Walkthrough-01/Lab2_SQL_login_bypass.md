🎯 Goal of the Lab
Login to the application as the administrator using a SQL injection attack.

🛠️ Tools You Need
A browser with Burp Suite set up as a proxy.

The lab URL provided by PortSwigger (click ACCESS THE LAB).

🧪 What You’re Exploiting
The login function likely runs this kind of SQL:

sql
Copy
Edit
SELECT * FROM users WHERE username = 'your_input' AND password = 'your_input';
You will inject SQL code into the username field to trick the query into always returning true — logging you in as administrator.

✅ Step-by-Step Solution (In Simple Words)
🔹 Step 1: Open the Lab in Your Browser
Click ACCESS THE LAB

You’ll see a normal login form (Username + Password)

🔹 Step 2: Turn on Burp Suite & Intercept
Open Burp Suite

Turn Intercept ON

In your browser, enter anything (e.g., test:test) into the login form and submit

🔹 Step 3: Capture the Login Request
In Burp, you'll see something like this:

http
Copy
Edit
POST /login HTTP/1.1
Host: your-lab-url
...

username=test&password=test
🔹 Step 4: Modify the Username to Inject SQL
Change this line:

http
Copy
Edit
username=test&password=test
To:

http
Copy
Edit
username=administrator'+OR+1=1--&password=anything
OR in raw form:

http
Copy
Edit
username=administrator' OR 1=1--&password=anything
Let’s break it down:

Part	Meaning
administrator'	Closes the original 'administrator' value
OR 1=1	Always true, so the query succeeds
--	Comments out the rest of the query (like password check)

So the backend SQL becomes:

sql
Copy
Edit
SELECT * FROM users WHERE username = 'administrator' OR 1=1 --' AND password = '...';
It ignores the password, and logs you in as administrator.

🔹 Step 5: Forward the Request
Click Forward in Burp

The lab should log you in as administrator

✅ Lab Solved!
You’ll see the “Congratulations, you solved the lab!” message.

🧠 Summary
You tricked the SQL query to skip password check

The injected query returns true, logging you in as administrator

SQL injection works when user input isn’t properly filtered or escaped
