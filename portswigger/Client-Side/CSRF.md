# CSRF
Attack that tricks a user's browser into executing an unwanted action on a web application where they are currently authenticated.

# CSRF Token
A CSRF token is a unique, secret, and unpredictable value generated server-side and shared with the client. It must be included in requests that perform sensitive actions to be considered valid.
SameSite Cookies

## CSRF Token Integration in HTML Forms

To mitigate CSRF (Cross-Site Request Forgery) attacks, it's common to include CSRF tokens within HTML forms. Below is an example of how a CSRF token can be embedded as a hidden field:

```
<form name="change-email-form" action="/my-account/change-email" method="POST">
  <label>Email</label>
  <input required type="email" name="email" value="example@normal-website.com">
  <input required type="hidden" name="csrf" value="50FaWgdOhi9M9wyna8taR1k3ODOR8d6u">
  <button class='button' type='submit'>Update Email</button>
</form>
```
When the form is submitted, the POST request sent to the server includes the CSRF token:
```
POST /my-account/change-email HTTP/1.1
Host: normal-website.com
Content-Length: 70
Content-Type: application/x-www-form-urlencoded

csrf=50FaWgdOhi9M9wyna8taR1k3ODOR8d6u&email=example@normal-website.com
```
It is recommended to place the CSRF token field early in the HTML document, ideally before any editable fields, to prevent data manipulation attacks that could alter the HTML structure.
Common CSRF Defenses
CSRF Tokens

## SameSite Cookies

SameSite cookies are a security measure used by browsers to restrict how cookies are sent with cross-site requests. This helps prevent CSRF attacks by ensuring that cookies are only sent under certain circumstances. 
```
Set-Cookie: session=0F8tgdOhi9ynR1M9wa3ODa; SameSite=Strict
Set-Cookie: trackingId=0F8tgdOhi9ynR1M9wa3ODa; SameSite=None; Secure
```
## Referer-Based Validation

This method uses the HTTP Referer header to verify requests originate from the same domain. Although it provides some level of security, it's generally less robust than token-based validation.
CSRF Attack Examples
CSRF Without User Interaction

An attacker can trigger actions without user interaction using hidden forms and JavaScript to automatically submit forms:
```

<html>
  <body>
    <form action="https://vulnerable-website.com/email/change" method="POST">
      <input type="hidden" name="email" value="pwned@evil-user.net" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```
## Bypassing CSRF Validation

Some applications do not enforce CSRF validation consistently across request methods, allowing attackers to exploit these loopholes:

-Omission of CSRF Token: If token validation is skipped when no token is present, attackers can simply omit the token.
-Global CSRF Token Pools: If tokens are not tied to user sessions, an attacker could use a token obtained from their own session in an attack.
-Mismatched CSRF and Session Cookies: When CSRF tokens are not tied to the session cookie, an attacker could manipulate cookies to mount an attack.
-If sensitive actions are permissible via GET requests, attackers can embed requests in images:

```
<img src="//vulnerable-website.com/email/change?email=pwned@evil-user.net">
```

