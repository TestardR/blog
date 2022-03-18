---
title: Cross-Site Request Forgery
date: '2019-02-13'
time: '☕️'
template: "post"
draft: false
slug: "CSRF-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "CSRF attack occurs when an attacker can use an HTTP
request to access a user’s information from another website, and use that information to
act on the user’s behalf."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What are Open Redirect Vulnerabilites

As said in the summary, a cross-site request forgery (CSRF) attack occurs when an attacker can use an HTTP
request to access a user’s information from another website, and use that information to
act on the user’s behalf.

This typically relies on the victim being previously authenticated on the target website where the action is submitted, and occurs without the victim
knowing the attack has happened.

## Cookies

Before talking in depth about CSRF, let's talk a little bit about cookies.

When you visit a website that requires authentication, like a username
and password, that site will typically store a cookie in your browser. Cookies are files
created by websites that are stored on the user’s computer.

Cookies can be used for various purposes such as for storing information like user preferences or the user’s history of visiting a website.
Furthermore, to store this information, cookies can have some attributes, which are standardized pieces of information that tell browsers
about the cookies and how they should be treated. Some attributes are the domain, expiry date, secure, and httponly attributes.

A site can set any number of cookies, each with their own purpose. For example, a site could use a session_id cookie to
remember who a user is rather than have them enter their username and password for every page they visit or action they perform.
Remember that HTTP is considered stateless meaning that with every HTTP request, a website doesn’t know who a user is, so it has
to re-authenticate them for every request.

### Secure & httponly

The secure and httponly attributes tell browsers when and how cookies can be sent and
read. These attributes don’t contain values, but instead act as flags that are either present
in the cookie or are not.

When a cookie contains the secure attribute, browsers will only
send that cookie when visiting HTTPS sites. If you visited http://www.site.com/ with a secure
cookie, your browser wouldn’t send your cookies to the site. This is to protect your privacy
since HTTPS connections are encrypted and HTTP ones are not.

The httponly attribute tells the browser that the cookie can only be read through HTTP and HTTPS requests. If a cookie is httponly, browsers won’t allow any scripting languages,
such as JavaScript, to read its value.

## Our story of a CSRF Attack

Let us consider the following example: Alice goes on its bank website to check her accounts.

When Alice visits her banking site and logs in, the bank will
respond to her HTTP request with an HTTP response, which includes a cookie identifying
Alice. In turn, Alice browser will automatically send that cookie with all other HTTP
requests to the banking website.

After finishing her banking, Alice doesn’t log out when she decides to visit https://www.gmail.com/.
This is important because when you log out of a site, that site will typically send an HTTP
response that expires your cookie.

When Alice visits the unknown site, she visits a malicious website, which
is designed to attack her banking website.

## CSRF with GET requests

If the banking site accepts GET requests, the attacker site will send the HTTP request
with a hidden form or an <img> tag.

When an <img> tag is rendered by a browser, it will make an HTTP GET request to the
src attribute in the tag. So, if the malicious site were to use a URL that transferred \$100
from Alice to Martin that looked like:

```
<img src="https://www.bank.com/transfer?from=alice&to=martin&amount=100">

```

As a result, when Alice visits the attacker's site, it includes the <img> tag in its HTTP
response and the browser then makes the HTTP GET request to the bank. The browser
sends Alice’s authentication cookies to get what it thinks should be an image when in fact
the bank receives the request, processes the URL in the tag’s src attribute, and processes
the transfer.

## CSRF with POST requests

The most simplistic situation involves a POST request with the content-type application/x-www-form-urlencoded or text/plain. The content-type is a header that browsers may include when sending HTTP requests.
It tells the recipient how the body of the HTTP request is encoded.

It’s possible for a malicious site to create a hidden HTML form and submit it silently to the target site without a victim knowing.

```
<iframe style="display:none" name="csrf-frame"></iframe>
<form method='POST' action='http://bank.com/transfer.php' target="csrf-frame" id\
="csrf-form">
<input type='hidden' name='from' value='Alice'>
<input type='hidden' name='to' value='Martin'>
<input type='hidden' name='amount' value='100'>
<input type='submit' value='submit'>
</form>
<script>document.getElementById("csrf-form").submit()</script>
```

Since the attacker doesn’t want Alice to see the
form, each of the < input> elements are given the type ‘hidden’ which makes them invisible
on the web page Bob sees. As the final step, the attacker includes some JavaScript inside
a < script> tag to automatically submit the form when the page is loaded.

Here, the browser makes the HTTP POST request to send Bob’s
cookies to the bank site, which invokes a transfer. Since POST requests send an HTTP
response back for the browser, the attacker hides the response in an < iframe> with the
display:none attribute so Bob doesn’t see it and realize what has happened.

## Defenses against CSRF Attacks

1. The most popular protection against CSRF is likely the CSRF token, which would be
   required by the protected site when submitting potentially data altering requests (that
   is, POST requests). These tokens aren’t always obviously named, but some potential examples of names include X-CSRF-TOKEN, lia-token, rt, or form-id.

2. The obvious other way sites protect themselves is by using CORS. Cross-origin resource sharing (CORS)
   is a mechanism that allows restricted resources on a web page to be requested from another domain outside the domain from which the first resource was served.

3. Lastly, CSRF vulnerabilities can also be avoided if a site validates the origin header
   submitted with an HTTP request, as the origin can’t be attacker-controlled and refers
   to the location where the request originated.
