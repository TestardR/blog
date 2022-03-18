---
title: CRLF Injection
date: '2019-02-15'
time: '☕️'
template: "post"
draft: false
slug: "CRLF-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "A Carriage Return Line Feed (CRLF) Injection vulnerability occurs when an application
does not sanitize user input correctly and allows for the insertion of carriage returns
and line feeds denote line breaks and have special significance."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What are CRLF injections ?

As said in the summary, Carriage Return Line Feed (CRLF) Injection vulnerability occurs when an application
does not sanitize user input correctly and allows for the insertion of carriage returns and line feeds denote line breaks and have special significance.

IFor example, HTTP message parsing relies on CRLF characters to identify sections of HTTP messages, including headers, as defined in RFCs and relied on by browsers. URL
encoded, these characters are %0D%0A, which decoded represent \r\n. The effect of a CRLF Injection includes HTTP Request Smuggling and HTTP Response Splitting.

### HTTP Request Smuggling

HTTP Request Smuggling occurs when an HTTP request is passed through a server
which processes it and passes it to another server, like a proxy or firewall. This type
of vulnerability can result in:

1. Cache poisoning, a situation where an attacker can change entries in an applica-
   tion’s cache and serve malicious pages
2. Firewall evasion, where a request can be crafted using CRLFs to avoid security
   checks
3. Request Hijacking, a situation where an attacker can steal HttpOnly cookies and
   HTTP authentication information

### HTTP Response Splitting

HTTP Response Splitting, however, allows an attacker to insert HTTP response headers
and potentially control HTTP response bodies or split the response entirely, effectively
creating two separate responses. This is effective because modifying HTTP headers can
result in unintended behavior, such as redirecting a user to an unexpected website or
serving explicitly new content controlled by attackers.
