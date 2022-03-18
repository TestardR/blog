---
title: What about security specific to Javascript ?
date: "2018-12-12"
time: '☕️'
template: "post"
draft: false
slug: "JavaScript-security"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "We take a look a security risks involved with managing Auth Tokens, Cache Storage and Service Workers, and Third-party Library Vulnerabilities."
---

This article was done using my notes from a brilliant talk, between Troy Hunt and Aaron Powell in 2018.

<sub>Hunt T. & Powell A. (2018). "Play by Play: JavaScript Security", Pluralsight, https://app.pluralsight.com/player?course=play-by-play-javascript-security&author=troy-hunt&name=31118774-8b2e-45f7-bbab-928ddc923448&clip=3&mode=live</sub>

## Managing Auth Tokens

Modern login and registration forms use authorization tokens, known as auth tokens. They are given to the client by the server. They allow for permission handling within the application. Auth tokens usually persist on the client allowing the user to refresh its browser without being logged out.

Traditionally, auth token would sit in a cookie. If you did not have a cookie, it would probably be in the URL, which is terrible for all sort of reasons. Cookies are great, they allow a lot of things, such as auto-expiry of the auth token. However, cookies have a downside. To make them secure, you have to turn them into HttpOnly cookies. When a cookie is flagged HttpOnly, it can't be accessed by a client script. When you call for async services and need to send an auth token, you have to be able to access it, which means it can't be HttpOnly. So securing cookies makes them unusable.

To circumvent this issue with cookies, we have other possibilities. Browser have built in storage. Session storage is entirely owned by a particular browser window or tab. As soon as we close the window or tab, it kills the session storage. It's a little bit like a cookie expiring at the end of the session. However, cookies have a precise expiration date, which is not the case for session storage. On the upside, auth tokens do not get sent with every request like a cookie. On the down side, we can't protect it from a client script, such as XSS, as we would do with a HttpOnly cookie.

To remember identities between sessions, we would use local storage. Session storage and local storage are pretty similar, except session storage is here for the lifetime of your browser. An auth token would persist even though you closed and opened another browser window.

## Cache Storage and Service Workers

Service workers are very popular, they are appearing on all browsers, such as Chrome, Firefox, iOS, and Android, allowing developers to build progressive web applications. A service worker runs in the background of a web application and continues to run even when you don't have a browser open, background processing. It is used to do things like intercept network requests and store the response in the cache storage.

Like session and local storages, cache storage can be accessed by anything that is running on your page context as it is a global object, including attacker scripts (XSS). To protect your cache storage, you have to run it through HTTPS or localhost, which is considered as a secure context.

## Third-party Library Vulnerabilities

Many developers import libraries to their applications through CDN (Content Delivery Network) or services in the same manner. Owners of libraries can merely do whatever they want with them. Modify them without you knowing exactly what are the changes are about. A good way to deal with content that you import on your application is through CSP (Content Security Policy). After adding CSP to your application and setting it according to your needs. It will make sure that the libraries brought on your application have not been tempered with.

NPM packages is a source of vulnerabilities. More and more, developers are downloading dependencies through NPM package manager to run their applications. People are downloading a lot of content through NPM, because its the fast track to building applications. But what happens when you can't trust what you downloaded? Snyk.io is a site you can look at to find out about vulnerabilities in your dependencies. Typosquatting is the practice of masquerading as another similar package to push malicious content. Be aware that you might download malicious dependencies mistyping for the one you were really looking for...
