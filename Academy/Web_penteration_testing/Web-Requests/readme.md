Web Requests

Overview & Architecture
HyperText Transfer Protocol (HTTP)

HTTP is a stateless client-server communication protocol operating primarily over TCP port 80 (or 443 for HTTPS). The client (typically a browser, cURL, or an automated script) sends an HTTP request to a web server, which processes the request and serves back a response containing headers and a body payload.

Clients target resources using Fully Qualified Domain Names (FQDNs) or direct IP addresses, structured as Uniform Resource Locators (URLs).

![Alt text description](url_structure.png)


Key Components & Specifications
1. URL Breakdown

A Uniform Resource Locator (URL) uniquely identifies a resource across a network and consists of several distinct parts:

Scheme: Specifies the protocol used to access the resource (e.g., http:// or https://).

User Info: An optional field containing credentials formatted as username:password@ used to authenticate directly to the host.

Host: Identifies the location of the resource using a hostname, FQDN, or IP address (e.g., inlanefreight.com).

Port: An optional numerical port number separated by a colon (e.g., :80 or :443). If omitted, it defaults to standard protocol ports.

Path: Points to the specific directory or file resource on the server (e.g., /dashboard.php). It defaults to index pages if empty.

Query String: Begins with a question mark (?) and contains key-value pairs separated by ampersands (&) to pass parameter data to the server.

Fragment: Begins with a hash (#) and is processed client-side by the browser to locate or scroll to a specific section within the resource.

2. HTTPS & TLS 1.3

Standard HTTP transfers all data in plaintext, making it vulnerable to interception and tampering. HTTPS wraps HTTP traffic inside Transport Layer Security (TLS) to guarantee confidentiality via encryption and integrity via cryptographic signing. Modern TLS 1.3 reduces handshake latency to a single round-trip time (1-RTT) by performing server authentication, key exchange (using Ephemeral Diffie-Hellman), and session negotiation simultaneously.
3. HTTP Methods & RESTful Mapping

HTTP methods (or verbs) define the intended action on a server resource, mapping directly to CRUD (Create, Read, Update, Delete) database operations:

GET (Read): Requests a specific resource. Parameters are passed within the URL query string. It is idempotent (repeated requests yield the same state).

POST (Create): Sends data in the HTTP request body (such as form data, JSON, or file uploads) to create or process a resource.

HEAD (Read): Functions identically to GET, but returns only response headers without the payload body.

PUT (Update): Fully replaces or creates a target resource at the specified URI.

PATCH (Update): Applies partial modifications to an existing resource.

DELETE (Delete): Removes the specified resource from the server.

OPTIONS: Queries server capabilities, supported HTTP methods, and Cross-Origin Resource Sharing (CORS) policies.

4. HTTP Status Codes

Status codes signal the result of an HTTP request and fall into five main categories:

1xx (Informational): Indicates the request was received and processing continues (e.g., 100 Continue).

2xx (Success): Confirms the request was successfully received, understood, and accepted (e.g., 200 OK, 201 Created).

3xx (Redirection): Directs the client to take additional action or follow a new URL (e.g., 301 Moved Permanently, 302 Found).

4xx (Client Error): Highlights client-side issues such as bad syntax, invalid permissions, or missing pages (e.g., 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found).

5xx (Server Error): Signals that the server failed to fulfill a valid request due to an internal error or gateway issue (e.g., 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable).

HTTP Headers Taxonomy
General & Entity Headers

Date: Displays the UTC timestamp when the message originated.

Connection: Controls connection persistence (e.g., keep-alive or close).

Content-Type: Declares the media format and encoding of the body payload (e.g., application/json; charset=UTF-8).

Content-Length: Indicates the total payload size in bytes.

Content-Encoding: Specifies any compression applied to the payload (e.g., gzip).

Request Headers

Host: Specifies the target domain name. Crucial for virtual hosting (vhosts).

User-Agent: Identifies the operating system, browser, or client software sending the request.

Referer: Shows the address of the web page that linked to the requested URI.

Accept: Informs the server which media types the client can handle.

Cookie: Sends stored session state or authentication tokens back to the server.

Authorization: Transmits credentials via schemes like Basic Auth or Bearer tokens.

Response & Security Headers

Server: Discloses server software and operating system version details.

Set-Cookie: Issues session identifiers or parameters to the client browser.

WWW-Authenticate: Specifies the authentication scheme required to access a protected resource.

Content-Security-Policy (CSP): Mitigates cross-site scripting (XSS) by restricting where scripts can load from.

Strict-Transport-Security (HSTS): Enforces strict HTTPS communication for future connections.

Practical Tooling & Workflows
Browser DevTools

Network Tab (Ctrl + Shift + E): Inspects raw HTTP requests and responses, timing, headers, payloads, and lets you copy requests as cURL commands.

Storage Tab (Shift + F9): Manages stored cookies (such as PHPSESSID), Local Storage, and Session Storage.

Console Tab (Ctrl + Shift + K): Executes custom JavaScript code and fetch() requests directly within the current browser session.

Key cURL Commands

    Basic Request: curl -v http://<IP>:<PORT>/ inspects connection details and headers.

    File Download: curl -s http://<IP>:<PORT>/download.php -o file.bin downloads binary or text output.

    Header Modification: curl -H "User-Agent: custom-agent" http://<IP>:<PORT>/ spoofs request headers.

    Authentication: curl -u admin:admin http://<IP>:<PORT>/ passes Basic Auth credentials.

     POST Form Data: curl -X POST -d "param1=value1&param2=value2" http://<IP>:<PORT>/ submits form data.

    Authenticated JSON POST: curl -X POST -b "PHPSESSID=<TOKEN>" -H "Content-Type: application/json" -d '{"key":"value"}' http://<IP>:<PORT>/ sends JSON payloads with session cookies.

    API Operations: curl -X PUT updates resources, curl -X DELETE removes resources, and curl -s piped to jq formats JSON responses.



HTB Module Questions & Walkthrough Solutions

Question 1: File Download via cURL

To get the flag, start the above exercise, then use cURL to download the file returned by '/download.php' in the server shown above.

    curl $IP:$PORT/download.php
    ls #to see the downloaded file name
    cat #downloaded file

    HTB{64$!c_cURL_....} write the commands above to get a flag like this :)


HTTP Requests and Responses

What is the HTTP method used while intercepting the request? (case-sensitive)
    A/ GET

    
Send a GET request to the above server, and read the response headers to find the version of Apache running on the server, then submit it as the answer. (answer format: X.Y.ZZ)

        curl -I http://$IP:$PORT/
        
2.4.??    something like this

HTTP Headers

The server above loads the flag after the page is loaded. Use the Network tab in the browser devtools to see what requests are made by the page, and find the request to the flag.

The backend filters responses based on User-Agent. Browsers receive dummy output, while cURL requests yield the result.

HTB{p493_r3qu3$t$_m0n!???} ** Check the network tab in your browser


GET

The exercise above seems to be broken, as it returns incorrect results. Use the browser devtools to see what is the request it is sending when we search, and use cURL to search for 'flag' and obtain the flag. 

        curl -s "http://$IP:$PORT/search.php?search=flag" -H 'Authorization: Basic YWRtaW46YWRtaW4='

        Answer: HTB{curl_g377??} WRITE THE COMMAND ABOVE IN YOUR TERMINAL

POST

Obtain a session cookie through a valid login, and then use the cookie with cURL to search for the flag through a JSON POST request to '/search.php' 

1: first login with credantials provided
2: then using the browser devtool go to storage > cookies
or just from the network tab right click on the request and copy Request Header
or just use curl with -i for viewing the header 
3: finally using curl to capture the flag

        curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=YOURCOCKIE' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php

CRUD API

Question : First, try to update any city’s name to be ‘flag’. Then, delete any city. Once done, search for a city named ‘flag’ to get the flag.

ANSWER
1: as requested update a name of any city to flag and verify if it is updated
2: delete a city and verify if it is deleted
3: finally capture the flag

    HTB{crud_4p!_m4n!pul4t0r}

