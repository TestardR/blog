---
title: Buffer Overflows and Memory Corruptions
date: '2019-03-01'
time: '☕️'
template: "post"
draft: false
slug: "Memory-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "Buffer Overflows and Memory Corruptions are two attacks in the area of
memory. They both lead to erractic program behaviour."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

## What is a Buffer Overflow ?

As said in the summary, a Buffer Overflow is a situation where a program writing data to a buffer, or area of
memory, has more data to write than space that is actually allocated for that memory. Buffer Overflows lead to erratic program behaviour at best and a serious security
vulnerability at worst.

The reason is, with a Buffer Overflow, a vulnerable program
begins to overwrite safe data with unexpected data, which may later be called upon.
If that happens, that overwritten code could be something completely different that the
program expects which causes an error. Or, a malicious hacker could use the overflow
to write and execute malicious code.

According to OWASP (2019), Buffer overflow flaws can be present in both the web server and application server products that serve the static and dynamic portions of a site, or in the web application itself.

<sub>OWASP. (2019). Buffer Overflows, url : https://www.owasp.org/index.php/Buffer_Overflows</sub>

### Read data ouf of bounds

In addition to writing data beyond the allocated memory, another vulnerability lies in
reading data outside a memory boundary. This is a type of Buffer Overflow in that
memory is being read beyond what the buffer should allow.

A famous and recent example of a vulnerability reading data outside of a memory bound-
ary is the OpenSSL Heartbleed Bug, disclosed in April 2014. At the time of disclosure, ap-
proximately 17% (500k) of the internet’s secure web servers certified by trusted authori-
ties were believed to have been vulnerable to the attack (Dewey, 2014).

<sub>Dewey C. (2014). Why is it called the 'Hearthbleed Bug'?, Washington Post, url : https://www.washingtonpost.com/news/arts-and-entertainment/wp/2014/04/09/why-is-it-called-the-heartbleed-bug/?noredirect=on&utm_term=.8a83b3b490d4</sub>

## What is a Memory Corruption

Memory corruption is a technique used to expose a vulnerability by causing code to
perform some type of unusual or unexpected behaviour. The effect is similar to a buffer
overflow where memory is exposed when it shouldn’t be.

An example of this is Null Byte Injection (OWASP, 2019). This occurs when a null byte, or empty string
%00 or 0x00 in hexidecimal, is provided and leads to unintended behaviour by the
receiving program. In C/C++, or low level programming languages, a null byte represents
the end of a string, or string termination. This can tell the program to stop processing
the string immediately and bytes that come after the null byte are ignored.

Now, with regards to web applications, this becomes relevant when web applications
interact with libraries, external APIs, etc. written in C. Passing in %00 in a Url could lead
to attackers manipulating web resources, including reading or writing files based on the
permissions of the web application in the broader server environment. Especially when
the programming language in question, like PHP, is written in a C programming language
itself.

<sub>OWASP. (2019). OWASP Periodic Table of Vulnerabilities - Null Byte Injection, url : https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities_-_Null_Byte_Injection</sub>
