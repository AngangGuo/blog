---
title: "Notes - HTMX"
date: 2024-08-02T11:02:05-07:00
categories:
  - Web
tags:
  - HTMX
  - AJAX
draft: false
---

### AJAX vs SSE
* SSE: the client establishes a persistent and long-term connection with the server. The server uses this connection to send data to a client.
* AJAX: the client repeatedly polls (or requests) a server for data. The client makes a request and waits for the server to respond with data. If no data is available, an empty response is returned.

How to Choose?
* Use SSE: For real-time data streams where constant updates are crucial (e.g., live chat, stock tickers).
* Use AJAX: For scenarios requiring two-way communication, or data exchange beyond real-time updates (e.g., shopping carts, form submissions).

## Troubleshooting
### Method is not allowed(405)
```
// code inside web page <script> tag
const source = new EventSource("/sse");
```
Running the above code get the following error message(From browser developer > Source):
```
Failed to load resource: The server response with a status of 405(Method Not Allowed)
```

Solution:
`EventSource()` is a `GET` method, if the server only has `POST` method you'll get this error, 
even if the link is right. The server must respond to the GET method.
