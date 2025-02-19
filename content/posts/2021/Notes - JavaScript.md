---
title: "Notes - JavaScript"
date: 2021-02-22T07:10:14-08:00
categories:
- Tech
- Program
- Web
tags:
- JavaScript
draft: false
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

## JavaScript
### URL functions
![url diagram](/images/2021/url-diagram.JPG)

```
// window.location = window.location.href
console.log(window.location.href) // https://www.w3schools.com/jsref/tryit.asp?filename=tryjsref_split1
console.log(window.location.origin) // https://www.w3schools.com
console.log(window.location.host) // www.w3schools.com

const url = new URL("https://www.example.com/path/to/page?query=value#fragment");
console.log(url.protocol); // "https:"
console.log(url.host);     // "www.example.com"
console.log(url.pathname); // "/path/to/page"
console.log(url.search);   // "?query=value"
console.log(url.hash);     // "#fragment"
```

See [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/API/URL)

### Template literals
In JavaScript, you can use variables in strings by using a technique called string interpolation or template literals. 
Template literals are enclosed by backticks “ ``` ` ``` “ (instead of single or double quotes) and use the syntax ``` `${variable}` ``` 
to insert the variables’ values inside the string. Here’s an example:

```
let name = 'Andrew';
console.log(`My name is ${name}.`);
```

## Using XPath in JavaScript
See [Introduction to using XPath in JavaScript](https://developer.mozilla.org/en-US/docs/Web/XPath/Introduction_to_using_XPath_in_JavaScript)
```javascript
var xpathResult = document.evaluate( xpathExpression, contextNode, namespaceResolver, resultType, result );
```

## Async / Await

## Event
```javascript
document.getElementById('name')
  .addEventListener('keyup', function(event) {
    if (event.code === 'Enter') {
      event.preventDefault();
      document.querySelector('form').submit();
    }
  });
```

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

// getData().then().catch()
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

### How to post form data by using JavaScript?
```
<form name="myform1" action="/save" method="post">
    <input type="submit" value="Save" onclick="doSubmit()" id="myButton">
</form>

<script type="text/javascript">
    function doSubmit() {
        var btn = document.getElementById("myButton");
        btn.value = "Waiting ..."
        btn.disabled = true;
        document.forms["myform1"].submit();
    }
</script>
```

### How to prevent double click/submit?
```
<script>
    var clicked = false;
    function doSubmit2() {
        if (!clicked) {
            clicked = true;

            var btn = document.getElementById("myButton2");
            btn.value = "Waiting 2 ..."
            btn.disabled = true;
            document.forms["myform2"].submit();
        }
        // double click: do nothing
    }
</script>
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

### Fetch Errors
`fetch()` doesn't throw an error when the server returns a bad HTTP status, e.g. client (400–499) or server errors (500–599).

`fetch()` rejects only if a request cannot be made or a response cannot be retrieved. 
It might happen because of network problems: no internet connection, host not found, the server is not responding.
```
if (!response.ok) {
    const message = `An error has occured: ${response.status}`;
    throw new Error(message);
}
```
