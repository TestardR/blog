---
title: Template Injection
date: '2019-02-17'
time: '☕️'
template: "post"
draft: false
slug: "TI-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "Template injections occur when engines render user input without properly sanitizing it.Template injection vulnerabilities can sometimes lead to remote code execution."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What is a Template Injection ?

A template engine is code used to create dynamic websites, emails, and so on. The basic
idea is to create templates with dynamic placeholders for content. When the template
is rendered, the engine replaces these placeholders with their actual content so that the
application logic is separated from presentation logic.

There are two types of template injection vulnerabilities, server side and client side. Both occur when engines render user input without properly sanitizing it, similar to cross-
site scripting. However, unlike cross-site scripting, template injection vulnerabilities can sometimes lead to remote code execution.

## Server Side Template Injections

Server side template injections, also know as SSTIs, occur when the injection happens
in the server side logic. Since template engines are usually associated with specific
programming languages, when an injection occurs, it may be possible to execute
arbitrary code from that language. The ability to execute code depends on the security
protections provided by the engine as well as preventative measures the site may have
taken.

The syntax for testing SSTI depends on the engine being used but typically involves
submitting template expressions with a specific syntax. For example, the PHP template engine Smarty uses four braces ({{ }}) to denote expressions whereas erb uses a combination of brackets, percent symbols, and an equal sign (<%= %>).

Since the syntax isn’t uniform across all templating engines, it’s important to determine
what software was used to build the site being tested. Tools like Wappalyzer or BuiltWith
are specifically designed to help do this.

## Client Side Template Injections

Client Side Template Injections, or CSTI, are a result of template engine injections which
occur in client template engines, typically written in JavaScript. Popular client template
engines include AngularJS developed by Google and ReactJS developed by Facebook.

Since CSTI injections occur in the software executing in the user’s browser, most
injections can typically only be exploited to achieve cross-site scripting (XSS) and not
remote code execution.

However, achieving XSS can sometimes be difficult and require
bypassing preventative measures. However, ethical hackers routinely found and released Angular sandbox bypasses. A
popular bypass used for the Sandbox in versions 1.3.0-1.5.7 that you can submit when a
Angular injection is found is:

```
{{a=toString().constructor.prototype;a.charAt=a.trim;$eval('a,alert(1),a')}}

```
