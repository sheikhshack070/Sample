# DVWA Security Lab Report

## Student: Sheikh Sabahat
## Course: Application Security Testing  
## Environment: DVWA deployed using Docker

---


# Vulnerability Testing

## Vulnerability: Brute Force

The Brute Force vulnerability demonstrates how attackers can attempt multiple password guesses until they successfully authenticate.

DVWA allows testing this vulnerability at three security levels:

- Low
- Medium
- High

---

# Brute Force — Security Level: LOW

## Payload Used

```
password
```

Username used:

```
admin
```

Example login request:

```
http://localhost:8080/vulnerabilities/brute/?username=admin&password=password&Login=Login
```

---

## Result

The login was successful when the correct password was entered.

There were no restrictions on the number of login attempts, meaning multiple password guesses could be tried repeatedly.

---

## Screenshot

```
screenshots/bruteforce_low_success.png
```

*(Insert screenshot showing successful login)*

---

## Explanation (Why the Attack Works)

At the **Low security level**, DVWA does not implement any protection mechanisms against brute force attacks.

User input is directly processed without restrictions such as:

- account lockouts
- login attempt limits
- CAPTCHA verification
- request delays

Because of this, attackers can continuously try different password combinations until the correct credentials are discovered.

# Brute Force — Security Level: MEDIUM

## Payload Used

```
password
```

Username used:

```
admin
```

---

## Result

The login still succeeds when the correct password is entered, but the system introduces a delay between login attempts.

---

## Screenshot

```
screenshots/bruteforce_medium.png
```

*(Insert screenshot of the login attempt)*

---

## Explanation (Why the Attack Still Works)

At the **Medium security level**, DVWA introduces a small delay after each login attempt.

This delay slows down brute force attacks but does not completely prevent them.

Attackers can still perform brute force attacks using automated tools, although the attack will take longer due to the delay.

The application still lacks critical protections such as:

- rate limiting
- account lockouts
- IP blocking

Therefore, the vulnerability remains exploitable.

---

# Brute Force — Security Level: HIGH

## Payload Used

```
password
```

Username used:

```
admin
```

---

## Result

At the High security level, the brute force attack becomes more difficult due to the implementation of additional protections.

---

## Screenshot

```
screenshots/bruteforce_high.png
```

*(Insert screenshot showing the login page with security controls)*

---

## Explanation (Why the Attack Becomes Difficult)

At the **High security level**, DVWA introduces additional protection mechanisms such as **CSRF tokens**.

Each login request requires a unique token that must match the user session.

Example request parameters include:

```
username=admin
password=password
user_token=unique_token_value
Login=Login
```

Because the token changes with each request, automated brute force scripts cannot easily repeat login attempts without retrieving a new token for every request.

This significantly reduces the effectiveness of automated brute force attacks.

---

# Security Level Comparison

| Security Level | Protection Implemented | Attack Success |
|----------------|-----------------------|---------------|
| Low | No protection | Attack succeeds easily |
| Medium | Delay between login attempts | Attack succeeds but slower |
| High | CSRF token protection | Attack becomes difficult |

---

# OWASP Top 10 Mapping

The brute force vulnerability relates to the following OWASP category:

**A07: Identification and Authentication Failures**

This occurs when authentication mechanisms fail to protect against credential guessing attacks.

---

# Command Injection

## Vulnerability Overview

Command Injection occurs when an application passes user-controlled input to a system shell command without proper validation or sanitization. This allows attackers to execute arbitrary commands on the server.

In DVWA, the user is asked to input an IP address to ping a device. The input is passed to a system command that executes the ping operation.

If the input is not properly sanitized, attackers can append additional system commands.

---

# Security Level: LOW

## Payload Used

```
127.0.0.1; ls
```

## Result

The application executed both the ping command and the injected `ls` command. The output displayed the list of files from the server directory.

## Screenshot

![Command Injection Low](screenshots/command_low.png)

## Explanation (Why it Worked)

At the Low security level, the application does not perform any validation or filtering of user input.

The backend command executed by the server resembles:

```
ping 127.0.0.1; ls
```

Because the semicolon (`;`) is interpreted by the shell as a command separator, the system executes both commands sequentially.

This allows attackers to run arbitrary commands on the server.

---

# Security Level: MEDIUM

## Payload Used

```
127.0.0.1 | ls
```

## Result

The application executed the injected command and displayed the output of the `ls` command.

## Screenshot

![Command Injection Medium](screenshots/command_medium.png)

## Explanation (Why it Worked)

At the Medium security level, DVWA attempts to filter certain dangerous characters such as `;` and `&&`.

However, the filter does not block the pipe operator (`|`).

The pipe operator allows the attacker to chain commands, resulting in command injection despite the partial filtering.

This demonstrates how incomplete input filtering can still leave an application vulnerable.

---

# Security Level: HIGH

## Payload Used

```
127.0.0.1 | whoami
```

## Result

The command injection was still successful and returned the username of the process running the web server inside the container.

## Screenshot

![Command Injection High](screenshots/command_high.png)

## Explanation (Why it Worked)

Although the High security level introduces stronger filtering mechanisms, the application still allows the pipe (`|`) operator to be processed by the shell.

Because of this, attackers can still inject commands using alternative operators that bypass the implemented filters.

This highlights a common mistake in security implementations where developers attempt to blacklist certain characters instead of using proper input validation or safe command execution methods.

---

# Security Level Comparison

| Security Level | Payload | Result | Reason |
|----------------|--------|--------|-------|
| Low | `127.0.0.1; ls` | Successful | No input filtering |
| Medium | `127.0.0.1 | ls` | Successful | Partial filtering |
| High | `127.0.0.1 | whoami` | Successful | Pipe operator not blocked |

---

# OWASP Top 10 Mapping

This vulnerability falls under:

**OWASP Top 10: A03 – Injection**

Command injection is a type of injection vulnerability where user input is interpreted as part of a system command.

---

# Security Impact

If exploited in a real-world application, attackers could:

- Execute arbitrary system commands
- Read sensitive files from the server
- Escalate privileges
- Completely compromise the server

This makes command injection one of the most critical vulnerabilities in web applications.

---

