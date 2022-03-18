---
title: Remote Code Execution
date: '2019-02-27'
time: '☕️'
template: "post"
draft: false
slug: "RCE-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "Remote Code Execution refers to injecting code which is interpreted and executed by
a vulnerable application."
---


This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## How does RCE work ?

As said in the summary, Remote Code Execution (RCE) refers to injecting code which is interpreted and executed by
a vulnerable application. This is typically caused by a user submitting input which the
application uses without any type of sanitization or validation.

This could look like the following:

```
$var = $_GET['page'];
eval($var);
```

Here, a vulnerable application might use the url index.php?page=1 however, if a
user enters index.php?page=1;phpinfo() the application would execute the phpinfo()
function and return its contents.

Similarly, Remote Code Execution is sometimes used to refer to Command Injection
which OWASP (2018) differentiates. With Command Injection, according to OWASP, a vulnerable
application executes arbitrary commands on the host operating system. Again, this is
made possible by not properly sanitizing or validating user input which result in user
input being passed to operating system commands.
In PHP, for example, this would might look like user input being passed to the system()
function.

<sub>OWASP. (2018).Command Injection, url : https://www.owasp.org/index.php/Command_Injection</sub>
