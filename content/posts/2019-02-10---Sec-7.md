---
title: Open Redirect Vulnerabilities
date: '2019-02-10'
time: '☕️'
template: "post"
draft: false
slug: "ORV-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "An open redirect vulnerability occurs when a victim visits a URL for a given
website and that website instructs the victim’s browser to visit a different
URL, on a separate domain"
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What are Open Redirect Vulnerabilites

As said in the summary, an Open Redirect Vulnerability occurs when a victim visits a website through an URL. This website instructs the victims's browser to visit a different website, through a different URL (on separate domain).

```
https://www.google.com/?redirect_to=https://overcode.netlify.com

On going to www.google.com, you will be redirected to overcoded.netlify.com

```

If google was not validating that the redirect_to parameter was for one to their legitimate sites, this could lead to a vulnerability to an open redirect.
Indeed, on sending a GET request, we could retun a HTTP response instructing the visitor's browser to make a GET request to http://www.attacker.com.

## What to look out for ?

The Open Web Application Security Project (OWASP), which is a community dedicated to application security has listed this vunerability in their 2013 reports. Open redirects exploit the trust of a given domain to lure victims to an attacker's website. This can be used in phishing attacks to tric users into believing they are submitting precious data to a trusted website.

When searching for these types of vulnerabilities, you're looking for a GET request sent to the site you're testing, with a parameter specifying a URL to redirect to.
Finding them, often requires keen observation. Redirect parameters are sometimes easy to spot with names like redirect_to=, domain_name=, deckout_url=, and so on. Whereas other times they may have less obvious names like r=, u=, and so on.
