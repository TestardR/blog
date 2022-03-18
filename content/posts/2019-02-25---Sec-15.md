---
title: Server Side Request Forgery
date: '2019-02-25'
time: '☕️'
template: "post"
draft: false
slug: "SSRF-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "Server-side request forgery, or SSRF, is a vulnerability where an attacker is able to make a
server perform unintended network requests."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What is a SSRF ?

As said in the summary, Server-side request forgery, or SSRF, is a vulnerability where an attacker is able to make a
server perform unintended network requests. SSRF is similar to CSRF with one difference. While CRSF aims at a user, SSRF victim is the website itself.

### HTTP Request Location

Depending on how the website is organized, a server vulnerable to SSRF may be able to
make an HTTP request to an internal network or to external addresses.

Some larger websites have firewalls that prohibit external internet traffic from accessing
internal servers, for example, the website will have a limited number of publicly facing
servers that receive HTTP requests from visitors and send requests onto other servers
that are publicly inaccessible.

A common example of this are database servers, which are
often inaccessible to the internet. When logging into a site like this, you might submit a
username and password through a regular web form. The website would receive your
HTTP request and perform its own request to the database server with your credentials,
then the database server would respond to the web application server, and the web
application server would relay the information to you.

Vulnerable servers that allow attackers to control requests to internal servers may
expose private information. For example, an SSRF on the previous database example
might allow an attacker to send requests to the database server and retrieve information
they shouldn’t have access to.

### HInvoking GET Versus POST Requests

Once you confirm a SSRF can be submitted, you should confirm what type of HTTP
method can be invoked to exploit the site: GET or POST. POST requests may be more
significant because they may invoke state changing behavior if POST parameters can be
controlled. State changing behavior could be creating user accounts, invoking system
commands or executing arbitrary code depending on what the server can communicate
with. GET requests on the other hand are often associated with exfiltrating data.

### Blind SSRFs

After confirming where and how you can make a request, the next thing to consider is
whether you can access the response of a request. When you can’t access a response,
you have a blind SSRF. For example, an attacker might have access to an internal network
through SSRF, but can’t read HTTP responses to the internal server requests.

In some blind SSRFs, response times may reveal information about the servers being
interacted with.

### Leveraging SSRF

When attackers are not able to target internal systems, they can instead try to exploit SSRFs
that impact users. IOne way of doing this is to return an XSS
payload to the SSRF request, which is executed on the vulnerable site. Stored XSS
payloads are especially significant if they are easily accessed by other users.
