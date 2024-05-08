# Access Control:

Authentication identifies the user and confirms that they are who they say they are.
Session management identifies which subsequent HTTP requests are being made by that same user.
Access control determines whether the user is allowed to carry out the action that they are attempting to perform.

# Exploit
-Try robots.txt or watch source code for some leak

-The application makes subsequent access control decisions based on the submitted value. For example:
```
insecure-website.com/login/home.jsp?admin=true
insecure-website.com/login/home.jsp?role=1
```
-Try to search for role/roleid params. Or try to update your account  with roleid params and see if is accepted

-Some applications enforce access controls at the platform layer by restricting access to specific URLs and HTTP methods based on the user's role. For example an application might 
configure rules like the following:
DENY: POST, /admin/deleteUser, managers
Some frameworks also support various non-standard HTTP headers that can be used to override the URL in the original request, such as X-Original-URL and X-Rewrite-URL.
Or maybe the rules only block POST request, not GET ,HEAD and others. Change POST to POSTX and see what happen.

# Spring 5.3

Similar discrepancies can arise if developers using the Spring framework have enabled the useSuffixPatternMatch option. 
This allows paths with an arbitrary file extension to be mapped to an equivalent endpoint with no file extension. 
In other words, a request to /admin/deleteUser.anything would still match the /admin/deleteUser pattern. 
**Prior to Spring 5.3, this option is enabled by default.** 

-Some websites base access controls on the **Referer** header submitted in the HTTP request.
The Referer header is generally added to requests by browsers to indicate the page from which a request was initiated. 
example: If the Referer header contains the main /admin URL, then the request is allowed.







