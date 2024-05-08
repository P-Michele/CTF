# Clickjacking (UI Redressing)

Clickjacking is an attack where someone uses multiple transparent or opaque layers to trick a user into clicking on a button or link on another page when they were intending to click on the top-level page. This is typically done using a deceptive overlay created via an iframe.

## Defenses Against Clickjacking

## Content Security Policy (CSP) and X-Frame-Options

- **X-Frame-Options**: This HTTP response header is used to indicate whether a browser should be allowed to render a page in a `<frame>`, `<iframe>`, `<embed>`, or `<object>`. Sites can use this to avoid clickjacking attacks, by ensuring their content is not embedded into other sites.

- **Content Security Policy (CSP)**: This is a more flexible option compared to X-Frame-Options. CSP can specify which domains can frame the content. This is defined using the `frame-ancestors` directive, which can allow specific sites to frame the content:

```
Content-Security-Policy: frame-ancestors 'self' https://normal-website.com https://*.robust-website.com;
```
CSP is better because it validates each frame in the parent frame hierarchy, whereas X-Frame-Options only checks the top-level frame.

## Example of Basic Clickjacking Attack
```
<head>
  <style>
    #target_website {
      position: relative;
      width: 128px;
      height: 128px;
      opacity: 0.00001;
      z-index: 2;
    }
    #decoy_website {
      position: absolute;
      width: 300px;
      height: 400px;
      z-index: 1;
    }
  </style>
</head>
<body>
  <div id="decoy_website">
	...web content...
  </div>
  <iframe id="target_website" src="https://vulnerable-website.com"></iframe>
</body>
```
# Techniques to Prevent Clickjacking:

Ensure the current application window is the main or top window.
Make all frames visible.
Prevent interactions with invisible frames.
Intercept and alert potential clickjacking attempts to users.

# Bypass Using HTML5 iframe Sandbox:

Using the HTML5 sandbox attribute on iframes can effectively neutralize "frame buster" scripts by restricting the framed content from checking if it is in the top window. For example:
```
<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe>
```

Can bypass frame buster as:

```
if (window.top !== window.self) window.top.location.replace(window.self.location.href);
```

# Clickjacking Combined with XSS

If a site is vulnerable to XSS, clickjacking can be used to exploit this by triggering XSS when a user interacts with a seemingly innocuous iframe. This underscores the importance of protecting against both XSS and clickjacking vulnerabilities.

