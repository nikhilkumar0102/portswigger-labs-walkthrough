# Cross-Site Scripting (XSS)

- XSS is a security bug that lets hackers inject malicious scripts into websites. When other users visit the infected page, the script runs in their browser, stealing data or doing harmful actions.

## How XSS Happens (With Examples)
### 1. Stored XSS (Permanent Attack)
- Malicious script is saved on the website (like in a comment or forum post).
- Every visitor who sees the infected content gets hacked.

#### Example:

- A hacker posts this in a comment:

```
<script>alert("You've been hacked!")</script>
```

- Now, every user who loads the page sees an alert.

### 2. Reflected XSS (Temporary Attack)
- The attack is hidden in a URL or input field.

- Only works if the victim clicks a bad link.

#### Example:

- A phishing email contains:

```
https://example.com/search?q=<script>stealCookies()</script>
```

-If you click it, the script runs and steals your login cookies.

### 3. DOM-based XSS (No Server Needed)
- The attack happens inside the browser (no server storage).

- Example: A website takes a URL parameter and displays it without safety checks.

#### Example:

- Bad URL:

```
https://example.com#<script>alert('XSS')</script>
```

- If the website uses document.location.hash unsafely, the script runs.

## What Can Hackers Do with XSS?

✔ Steal cookies → Hack your accounts.

✔ Log keystrokes → Capture passwords.

✔ Redirect users → Send you to fake login pages.

✔ Deface websites → Show fake messages.

## How to Stop XSS?

#### 1. Escape User Input

Before showing user content, encode special characters `(<, >, ", ')`.

#### Example (PHP):

```
echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');
```

#### 2. Use Security Headers

Add Content Security Policy (CSP) to block unsafe scripts:
```
 Content-Security-Policy: script-src 'self';
```

#### 3. Sanitize HTML

Use libraries like DOMPurify (JavaScript) to clean inputs:

javascript
```
const clean = DOMPurify.sanitize(userInput);
```

#### 4. Secure Cookies
Mark cookies as HttpOnly so JavaScript can’t read them:

http
```
Set-Cookie: sessionId=123; HttpOnly; Secure
```

## Test for XSS

Try harmless payloads like:

```
<script>alert(1)</script>
```

- If an alert pops up, your site is vulnerable!

## Stay Safe!

✅ Never trust user input.

✅ Always sanitize & escape data.

✅ Use security headers.

