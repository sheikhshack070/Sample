# DVWA Security Lab Report

## Student: [Your Name]  
## Course: Application Security Testing  
## Environment: DVWA deployed using Docker

---

# Environment Setup

## Docker Installation Verification

Docker was verified using the following command:

```bash
docker --version
```

---

## Pulling the DVWA Image

```bash
docker pull vulnerables/web-dvwa
```

---

## Running the DVWA Container

```bash
docker run -d --name dvwa -p 8080:80 vulnerables/web-dvwa
```

---

## Verifying Running Containers

```bash
docker ps
```

This command confirmed that the DVWA container was successfully running.

---

## Accessing the Application

The application was accessed in a web browser:

```
http://localhost:8080
```

Login credentials:

```
Username: admin
Password: password
```

After login, the database was initialized using the **Create / Reset Database** button.

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

This makes the application highly vulnerable to automated brute force attacks using tools such as:

- Hydra
- Burp Suite Intruder
- custom scripts

---

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

