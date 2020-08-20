---
title: "Website Security With ReCAPTCHA"
date: 2020-07-18T23:09:46-07:00
categories:
 - Tech
 - Security
tags:
 - Website
 - reCaptcha
draft: false
---

reCAPTCHA is a free google service that protects your website from spam and abuse. 
reCAPTCHA uses an advanced risk analysis engine and adaptive challenges to keep automated software 
from engaging in abusive activities on your site. It does this while letting your valid users pass through with ease.

### Which version should I use?
There're two versions in use: reCAPTCHA v2 and reCAPTCHA v3. 

reCAPTCHA v3 returns a score(1.0 is very likely a good interaction, 0.0 is very likely a bot) for each request without user friction. 
The score is based on interactions with your site and enables you to take an appropriate action for your site. 

Note: reCAPTCHA tokens expire after two minutes. If you're protecting an action with reCAPTCHA, 
make sure to call execute when the user takes the action rather than on page load.

### How to add reCaptcha v2 into your web page?
Here is a sample page using reCaptcha:
```
<html>
  <head>
    <title>reCAPTCHA demo: Simple page</title>
    <script src="https://www.google.com/recaptcha/api.js" async defer></script>
  </head>
  <body>
    <FORM METHOD="POST" ACTION=https://example.com/post onsubmit="return check();" name="donation">
      <div class="g-recaptcha" data-sitekey="your_site_key"></div>
      <br/>
      <input type="submit" value="Submit">
    </form>
  </body>
</html>
```

### How to check the reCaptcha v2 response?
For web users, you can get the user’s response token in one of three ways:

* `g-recaptcha-response` POST parameter when the user submits the form on your site
* `grecaptcha.getResponse(opt_widget_id)` after the user completes the reCAPTCHA challenge
* As a string argument to your callback function if `data-callback` is specified in either 
the `g-recaptcha` tag attribute or the callback parameter in the `grecaptcha.render` method

To check the response from within your web page:
```
function check() {
    // check reCaptcha response
    let response = grecaptcha.getResponse();
    if(response.length === 0){
        alert("Please select [I'm not a robot]");
        return false
    }
    return true
}
```

### Enable reCaptcha v3
https://stackoverflow.com/questions/51507695/google-recaptcha-v3-example-demo

https://developers.google.com/recaptcha/docs/v3
```
// Load the JavaScript API with your sitekey.
<script src="https://www.google.com/recaptcha/api.js?render=reCAPTCHA_site_key"></script>

// Call grecaptcha.execute on each action you wish to protect.
   
<script>
     function onClick(e) {
       e.preventDefault();
       grecaptcha.ready(function() {
         grecaptcha.execute('reCAPTCHA_site_key', {action: 'submit'}).then(function(token) {
             // Add your logic to submit to your backend server here.
         });
       });
     }
</script>

```
### Resource
* Admin Console: https://www.google.com/recaptcha/admin
* Developer Guide: https://developers.google.com/recaptcha/intro
* Tutorial: https://product.hubspot.com/blog/quick-guide-invisible-recaptcha
