---
title: "Notes - JavaScript"
date: 2021-02-22T07:10:14-08:00
draft: true
---

## DOM
### getElementById()
Note: no "#" at the beginning of the id
```javascript
let element = document.getElementById("message");
```

### Get/Set InnerText
```javascript
const renderedText = htmlElement.innerText
htmlElement.innerText = string
```

## Async / Await

### Top
Await can only be used inside async functions. 
You can wrap it inside an anonymous sync function or use async function expression:
```javascript
// function
(async () => {
  const response = await fetch('https://example.org/json');
  ...
})();

// or use function expression
const start = async () => {...};
start();
```

### Catch Errors
#### Inside:
try.. catch..

### Outside
then.. catch..

### Can I await a function without async before it?
Yes, as long as it returns a promise.
```javascript
async function myAsync() {
  console.log('Inside of myfunction');
}

// You don't need to use async keyword on this function because
// we can simply returns the promise returned by myAsync
function start() {
  return myAsync();
}

// Call start
(async() => {
  console.log('before start');
  await start();
  console.log('after start');
})();
```

## Fetch

### Search Parameters
* Naive approach
```javascript
const city = "Rome";
const price = "200";
const myNaiveUrl = `https://www.example.dev/?city=${city}&price=${price}`;
```
* Robust approach
```javascript
const url = new URL("https://www.example.dev/");
url.searchParams.append("city", "Rome");
url.searchParams.append("price", "200");
```

### Fetch - Get
```javascript
async function getData() {
    let response = await fetch(url);
    
    if (response.ok) { // if HTTP-status is 200-299
      // get the response body (the method explained below)
      let json = await response.json();
      return json;
    } else {
      alert("HTTP-Error: " + response.status);
    }
}
```

### Fetch - Post
```javascript
async function postData() {
    const response = await fetch('https://example.com/post', {
    method: 'POST',
    headers: {
      'Accept': 'application/json',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({a: 1, b: 'Textual content'})
  });
  const content = await response.json();

  return content
}

getData().then().catch()
```

A typical fetch request consists of two await calls:
```javascript
let response = await fetch(url, options); // resolves with response headers
let result = await response.json(); // read body as json
```

Or, without await:
```javascript
fetch(url, options)
.then(response => response.json())
.then(result => /* process result */)
```

### Response
Response properties:

    response.status – HTTP code of the response,
    response.ok – true is the status is 200-299.
    response.headers – Map-like object with HTTP headers.

Methods to get response body:

    response.text() – return the response as text,
    response.json() – parse the response as JSON object,
    response.formData() – return the response as FormData object (form/multipart encoding, see the next chapter),
    response.blob() – return the response as Blob (binary data with type),
    response.arrayBuffer() – return the response as ArrayBuffer (low-level binary data),

Fetch options:

    method – HTTP-method,
    headers – an object with request headers (not any header is allowed),
    body – the data to send (request body) as string, FormData, BufferSource, Blob or UrlSearchParams object.
