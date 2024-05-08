# DNS Rebinding Attack Overview

## Introduction

DNS rebinding is a powerful attack technique that manipulates the domain name system (DNS) to bypass browser-based security models such as the same-origin policy (SOP) and Cross-Origin Resource Sharing (CORS). This can enable an attacker to interact with devices on a victim's local network.

## Attack Mechanism

1. **Malicious Website Setup**:
   - An attacker sets up a malicious website with specifically crafted JavaScript code. This site is published on the internet under the attacker's control.

2. **Initial DNS Response**:
   - When a user visits the malicious website, their browser performs a DNS lookup. The attacker’s DNS server responds with an IP address that it controls, along with a very short Time-To-Live (TTL). This ensures the DNS response won't be cached for long.

3. **Loading Malicious Content**:
   - The user's browser loads the content from the malicious website, which includes the execution of the embedded JavaScript.

4. **Rebinding DNS Request**:
   - The malicious JavaScript then triggers another DNS lookup for the same domain. At this point, the attacker modifies the DNS entry to point to a local IP address on the user’s network.

5. **Cache and Rebind**:
   - Due to the short TTL of the initial DNS response, the browser may query DNS again when the malicious JavaScript requests it. However, if the TTL has not yet expired, the browser uses the cached DNS response which now points to the local network IP.

6. **Bypassing Security Restrictions**:
   - With the DNS entry rebound to the local IP, the malicious script can make requests to local IP addresses, bypassing the browser's same-origin policy. This allows the attacker to access and interact with networked devices within the user's local environment, such as routers, webcams, or printers.

## Implications

DNS rebinding exposes devices and services on a local network to unauthorized access from the web. Devices typically protected by network firewalls become accessible, potentially allowing information leakage, service manipulation, or exploitation of local network vulnerabilities.

## Prevention Strategies

- **Configure DNS Servers**: Ensure that DNS servers do not allow fast-changing DNS records, or implement protections against unusual TTL values that could be indicative of DNS rebinding attempts.
- **Browser Security**: Modern browsers implement various security measures to prevent DNS rebinding attacks, but ensuring browsers are up-to-date is crucial.
- **Network Configuration**: Use internal DNS servers for local network resources, or configure devices to not respond to requests coming from outside the local network.
- **Application-Level Security**: Design applications and devices to require authentication and validation of requests, irrespective of the origin.
