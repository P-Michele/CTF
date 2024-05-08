# Authentication Vulnerabilities and Testing Techniques

## Enumerating Usernames and Passwords

### Different Response Indicators

Authentication systems can inadvertently reveal information about valid usernames or passwords through:

- **Response Time Variations**:
  Observe response times with different combinations of usernames and passwords. Longer times might indicate a processing difference that can be exploited.

- **Error Message Differences**:
  Different error messages for incorrect usernames vs. incorrect passwords can help an attacker determine which part of their guess was wrong.

- **Status Code Changes**:
  Variations in HTTP status codes in response to authentication attempts may reveal whether a username or password is correct.

## Bypassing Brute Force Protections

### Header Manipulation

- **X-Forwarded-For**:
  Modify the `X-Forwarded-For` header to spoof IP addresses and bypass IP-based blocking or rate limiting.

### Advanced Brute Force Techniques

- **JSON Array of Passwords**:
  Attempt to authenticate using a single request that includes an array of possible passwords, exploiting weak input handling on the server.

- **Rate Limit Testing**:
  Determine the request limit before triggering anti-brute-force protections like account lockouts, then stay below this threshold.

## HTTP Basic Authentication Vulnerabilities

- **Base64 Encoding**:
  Basic authentication uses a Base64 encoded string (`Authorization: Basic [base64(username:password)]`) which is vulnerable to interception (Man-in-the-Middle attacks) and brute force.

## Multi-Factor Authentication (MFA) Vulnerabilities

### Bypassing MFA

- **Token Validation**:
  Some systems do not properly verify the 2FA token. You might bypass this by redirecting to a personal page (e.g., `/my-account`) after the primary login, without completing the 2FA process.

- **Cookie Manipulation**:
  If a session cookie is set before 2FA completion, altering this cookie might trick the system into granting access without proper verification.

### Brute Forcing 2FA

- **Automated Retries**:
  If the system logs out users attempting to brute-force 2FA, use macros or scripts to automate login retries combined with 2FA code testing.

## Other Testing Methods

### Session Management

- **Persistent Login Weakness**:
  Test if the session management (e.g., "Remember Me" functionality) can be brute-forced or spoofed, typically via cookies or custom headers.

### Token Validation for Sensitive Actions

- **Account Deletion Tokens**:
  Check if tokens required for sensitive actions, like deleting an account or changing a password, are properly validated. Lack of rigorous checks can be exploited.

### Error Message Information Leakage

- **Password Change Forms**:
  Analyze error messages returned when entering existing passwords in "change password" forms. Consistency in error responses can be used to infer valid credentials, especially in systems with limited login attempts.

