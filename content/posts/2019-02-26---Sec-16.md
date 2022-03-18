---
title: XML External Entity Vulnerability
date: '2019-02-26'
time: '☕️☕️'
template: "post"
draft: false
slug: "XEE-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "An XML External Entity (XEE) attack is a type of attack against an application that parses XML input."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018, as well as reports made by OWASP.

<sub>OWASP. (2017). XML External Entity (XXE) Processing, url : https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Processing</sub>

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What is a XXE ?

As said in the summary, An XML External Entity attack (XXE) is a type of attack against an application that parses XML input.This attack occurs when XML input containing a reference to an external entity is processed by a weakly configured XML parser. This attack may lead to the disclosure of confidential data, denial of service, server side request forgery, port scanning from the perspective of the machine where the parser is located, and other system impacts.

### What is an eXtensible Markup Language (XML) ?

XML is a metalanguage used for describing other languages. It was developed after HTML in part, as a response to the shortcomings of HTML,
which is used to define the display of data, focusing on how it should look. In contrast,
XML is used to define how data is to be structured.

For example, in HTML, you have tags like < title>, < h1>, < table>, < p>, etc. all of which are
used to define how content is to be displayed. In contrast, XML has no predefined tags.
Instead, the person creating the XML document defines their own tags to describe the
content being presented. Here’s an example:

```
<?xml version="1.0" encoding="UTF-8"?>
<jobs>
<job>
<title>Hacker</title>
<compensation>1000000</compensation>
<responsibility optional="1">Shot the web</responsibility>
</job>
</jobs>
```

Since anyone can define any tag, the obvious question then becomes, how does anyone
know how to parse and use an XML document if the tags can be anything? A valid
XML document is valid because it follows the general rules of XML and it matches
its document type definition (DTD). The DTD is one of the things which can be exploited by attackers.

An XML DTD is like a definition document for the tags being used and is developed by
the XML designer, or author. A DTD will define which tags exist, what attributes
they may have and what elements may be found in other elements, etc. While you can create our own DTDs, some have been formalized and are widely used including
Really Simple Syndication (RSS), general data resources (RDF), health care information
(HL7 SGML/XML), etc.

Here’s what a DTD file would look like for my XML above:

```
<!ELEMENT Jobs (Job)*>
<!ELEMENT Job (Title, Compensation, Responsiblity)>
<!ELEMENT Title (#PCDATA)>
<!ELEMENT Compenstaion (#PCDATA)>
<!ELEMENT Responsibility(#PCDATA)>
<!ATTLIST Responsibility optional CDATA "0">
```

An XML entity is like a placeholder for information. The < jobs> tag is
actually an XML !ELEMENT and can contain the element Job. A Job is an !ELEMENT which
can contain a Title, Compensation and Responsibility, all of which are also !ELEMENTs
and can only contain character data, denoted by the (#PCDATA). Lastly, the !ELEMENT
Responsibility has a possible attribute (!ATTLIST) optional whose default value is 0.

### From XML to XXE ?

Using our previous example again,
if we wanted every job to include a link to our website, it would be tedious for us to
write the address every time, especially if our URL could change. Instead, we can use an
!ENTITY and get the parser to fetch the contents at the time of parsing and insert the
value into the document.

Our new DTD file woule thus looke like this:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE Jobs [
<!ELEMENT Job (Title, Compensation, Responsiblity, Website)>
<!ELEMENT Title (#PCDATA)>
<!ELEMENT Compenstaion (#PCDATA)>
<!ELEMENT Responsibility(#PCDATA)>
<!ATTLIST Responsibility optional CDATA "0">
<!ELEMENT Website ANY>
<!ENTITY url SYSTEM "website.txt">
```

And our XML file like this:

```
<jobs>
<job>
<title>Hacker</title>
<compensation>1000000</compensation>
<responsibility optional="1">Shot the web</responsibility>
<website>&url;</website>
</job>
</jobs>
```

Here, you’ll notice I’ve gone ahead and added a Website !ELEMENT but instead of
(#PCDATA), I’ve added ANY. This means the Website tag can contain any combination
of parsable data. I’ve also defined an !ENTITY with a SYSTEM attribute telling the parser
to get the contents of the website.txt file.

Putting this all together, what do you think would happen if instead of “website.txt”, I
included “/etc/passwd”? As you probably guessed, our XML would be parsed and the
contents of the sensitive server file /etc/passwd would be included in our content.

Well, an XXE attack is made possible when a victim application can be abused to include
such external entities in their XML parsing. In other words, the application has some
XML expectations but isn’t validating what it’s receiving and so, just parses what it gets.
To protect a site against XXE vulnerabilites. One has to disable the parsing of external entities.
