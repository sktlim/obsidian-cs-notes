> HyperText Transfer Protocol is the Web's **stateless** application layer protocol and defines the way that clients request web pages from servers (and vice versa)

## Properties
- HTTP is implemented in 2 programs
	- **Client program**: implemented in web browsers
	- **Server program:** implemented in web servers
- HTTP uses **TCP** as underlying protocol
- HTTP is a **stateless protocol** and it does not store state of client
- HTTP is a **pull protocol**: Someone loads their web server and uses HTTP to pull the relevant info

## Non-persistent and persistent connections
- **Non-persistent:** Each request/response takes place over **separate** TCP connection
- **Persistent:** All request/response of a particular interaction takes place over the same TCP connection

### HTTP with non-persistent connections
- **Note**: HTTP uses persistent connections by default
- Process
	1) HTTP client initiates TCP connection with server on port 80 with socket at client and at server
	2) HTTP client sends HTTP request message to server via its socket
	3) HTTP server process receives request message via its socket, retrieves object requested, encapsulates it in HTTP response message, and sends HTTP response message to client via client's socket
	4) HTTP server process tells TCP to close connection
	5) Process repeats for steps above
- In default mode, browsers open 5-10 parallel TCP connections, and each one handles one transaction (instead of the connections being done serially)
![[Example HTTP non-persistent request.png]]
- Total response time = 2 RTTs + transmission time by server

### HTTP with persistent connections
- Above method requires a brand new connection to be established and maintained every time which can introduce significant burden on the server at scale
- Each object also suffers delay of 2 RTTs + transmission time, which is undesirable
- With persistent connections, server leaves TCP connection open after sending a response
	- Requests can be pipelined (and are the default on HTTP)
	- HTTP/2 allows multiple requests and replies to be interleaved in the same connection, as well as a mechanism for prioritizing HTTP messages requests and replies within the connection

## HTTP Message Format
### Request Message
```
GET /index.html HTTP/1.1 
Host: www.example.com 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36 
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8 
Accept-Language: en-US,en;q=0.5 
Connection: keep-alive
```
- **Request Line (first line)**: 
	- Method field (See [[#HTTP Methods]])
	- URL field: Requested object
	- HTTP version field
- **Header Line**
	- **Host**: Specifies host on which object resides (required by web proxy caches)
	- **Connection**
		- `keep-alive`: Keep connection open (persistent connection)
		- `close`: Close connection (non-persistent connection)
		- `upgrade`: client request upgrade to different protocol e.g. WebSocket
	- **User-agent:** Specifies browser type that is making the request to the server
	- **Accept:** Content type accepted
	- **Accept-language:** Language that user is using in browser
- **Entity Body**
	- Empty for `GET` but used for `POST`
### Response Message
```
HTTP/1.1 200 OK
Date: Wed, 07 Nov 2024 12:30:00 GMT
Server: Apache/2.4.54 (Ubuntu)
Content-Type: text/html; charset=UTF-8
Content-Length: 123
Connection: keep-alive

{CONTENT}
```
- **Initial Status Line:**
	- Protocol version field
	- Status code (See [[#Additional Information]], point 4)
	- Status message 
- **Header Lines:**
	- **Date:** indicates date and time response was created and sent by server
	- **Server:** Analogous to `User-agent` in request
	- **Content-length**: in bytes
- **Entity Body**
## HTTP Methods

| **Method**  | **Description**                                                                                 | **Request Body** | **Idempotent** | **Safe** | **Use Cases**                                                   |
| ----------- | ----------------------------------------------------------------------------------------------- | ---------------- | -------------- | -------- | --------------------------------------------------------------- |
| **GET**     | Requests data from a server (read-only).                                                        | No               | Yes            | Yes      | Retrieving web pages, API data.                                 |
| **POST**    | Sends data to the server to create or update a resource.                                        | Yes              | No             | No       | Submitting forms, uploading files, creating resources via APIs. |
| **PUT**     | Updates or creates a resource by replacing its current representation with the request payload. | Yes              | Yes            | No       | Updating user profiles, replacing files.                        |
| **PATCH**   | Partially updates a resource, sending only the fields to be updated.                            | Yes              | Yes            | No       | Updating a single field in a database record.                   |
| **DELETE**  | Removes a resource from the server.                                                             | No               | Yes            | No       | Deleting user accounts, records, or files.                      |
| **HEAD**    | Same as GET, but retrieves only the headers (no body).                                          | No               | Yes            | Yes      | Checking resource metadata (e.g., size, modification date).     |
| **OPTIONS** | Describes the communication options for the target resource, including supported HTTP methods.  | No               | Yes            | Yes      | Preflight checks in CORS (Cross-Origin Resource Sharing).       |
| **TRACE**   | Echoes the received request to test connectivity and request routing.                           | No               | Yes            | No       | Debugging purposes to trace the path of a request.              |
| **CONNECT** | Establishes a tunnel to a server, typically for secure connections like HTTPS.                  | Yes              | No             | No       | Proxying connections through HTTP, often for SSL/TLS tunneling. |

### Additional Information:
2. **Safety**:
   - A method is **safe** if it doesn't modify server resources (e.g., `GET`, `HEAD`, and `OPTIONS`).

3. **Common Headers in HTTP**:
   - **Content-Type**: Specifies the type of data being sent or expected (e.g., `application/json`).
   - **Authorization**: Provides credentials for authenticating the client.
   - **Cache-Control**: Defines caching policies.
   - **Accept-Encoding**: Lists supported content encodings (e.g., `gzip`, `deflate`).

4. **Status Codes** (Grouped):
   - **1xx (Informational)**: Request received, continuing process (e.g., `100 Continue`).
   - **2xx (Success)**: Action successfully received, understood, and accepted (e.g., `200 OK`).
   - **3xx (Redirection)**: Further action needed to complete request (e.g., `301 Moved Permanently`).
   - **4xx (Client Error)**: Request contains bad syntax or cannot be fulfilled (e.g., `404 Not Found`).
   - **5xx (Server Error)**: Server failed to fulfill a valid request (e.g., `500 Internal Server Error`).

## Cookies
![[HTTP Cookies.png]]
- Due to stateless nature of HTTP server, info is not retained but there are circumstances where this might be useful
- HTTP uses cookies to allow sites to keep track of users
- **Components**
	- Cookie Header Line in response
	- Cookie header line in request
	- Cookie file kept in browser
	- Cookie kept in database of server

## Web Cache/ Proxy Server
> Network entity that satisfies HTTP requests on behalf of origin web server
![[Web Cache.png]]
- Web cache has its own disk storage and keeps copies of **recently requested objects** in storage
- User's browser can be configured such that it checks web cache first
	- If web cache does not have object, it opens a TCP connection to the origin server and HTTP requests the object, which it then stores and replies the client with
- Typically purchased and installed by an ISP
- **Advantages**
	- Substantially reduce response time for client request, especially if bottleneck bandwidth is large between client and origin server
	- Reduce traffic to internet, allowing for less upgrading constraints on the side of network infra (especially effective for institutional networks, where LAN is faster than access links to rest of internet)
- Typically used in [[Content Distribution Networks (CDN)]]

## Conditional GET
```
GET /fruit/kiwi.gif HTTP/1.1
Host: www.exotiquecuisine.com
If-modified-since: Wed, 9 Sep 2015 09:23:24
```
- Web caches introduce the problem of content being stale
- HTTP request message is a conditional `GET` if 
	- it uses `GET`
	- it contains header `If-Modified-Since`

**How it works**
1) On behalf of requesting browser, proxy cache sends request message to web server
2) Web server sends response message with requested object to cache
3) Cache forwards object to browser and stores object locally, along with its last-modified date
4) Cache does a conditional `GET` request to server when browser requests object again
	1) If not modified, web server will return a `304 Not Modified` to cache with empty entity body and cache forwards stored object to browser
	2) Else, server will return `200 OK` with entity body

