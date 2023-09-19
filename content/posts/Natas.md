---
title: "Natas"
date: 2023-09-11T10:47:20+05:30
draft: false
tags: [Web_App_Security, Hacking, Bandit, Overthewire]
image: https://github.com/Palash-Jain369/static/blob/main/Natas/Preview.png?raw=true
----
### [Natas](https://overthewire.org/wargames/natas/) is a wargame aimed at exploiting web application security vulnerabilities.

## Level0-->1:
View page source in browser either by right clicking or `ctrl + u` keyboard shortcut.

## Level 1-->2:
Many ways to solve. In browser, use shortcut mentioned above or disable java script in browser settings. Otherwise, use other utilities like `Curl, Burpsuite, Wireshark, python scripting... `

## Level2-->3:
![](https://github.com/Palash-Jain369/static/blob/main/Natas/Lvl2_natas.png?raw=true)Notice the URL. Try adding files to the url
`natas.........org/files`

## Level3-->4:
The comment about google/search-engines in the source code gives the hint that server is asking web crawlers to not index some pages. These pages are found in robots.txt file in the home directory of the server.

![](https://github.com/Palash-Jain369/static/blob/main/Natas/Lvl3.png?raw=true)Try adding disallowed name into URL.

## Level4-->5:
It seems like the server is checking where are we coming to the site from(hyperlinks). Such meta information about the request is contained in *HTTP Headers*. This information can be easily manipulated by a variety of methods and tools. Three prominent ones are CURL, Burpsuite, or a scripting language of your choice(python-requests) Here, I've used CURL command line utility.

![](https://github.com/Palash-Jain369/static/blob/main/Natas/Lvl5.png?raw=true)

`Command : curl -vH 'referer:http://natas5.natas.labs.overthewire.org/' -u natas4:tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm http://natas4.natas.labs.overthewire.org/`

## Level 5--6:
Information such as login is usually stored in cookies @ client machine. When clients make requests such as getting a web page, cookie is sent alongside, which allows server to manipulate content of web page conditionally. 

First check if server is indeed sending cookies by sending requests with CURL in verbose mode.
![](https://github.com/Palash-Jain369/static/blob/main/Natas/Lvl6_1.png?raw=true)Next step is to send the modified cookie and see if server is tricked....
![](https://github.com/Palash-Jain369/static/blob/main/Natas/Lvl6_2.png?raw=true)Comman used: 
`curl -v --cookie "loggedin=1" -u natas5:Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD http://natas5.natas.labs.overthewire.org/`

## Level6-->7:
Again a file inclusion.

## Level 7-->8:
It seems like the server is generating the content by taking actual filenames as parameters `...index.php?page=home`. Try passing 
```
natas...........org/about

```
This confirms our guess. It's clear that server is taking the value of 'page' parameter and navigating the file system for a file of this name, then rendering content of this file on the page.

```
natas ...org/index.php?page=../../../../etc/natas_webpass/natas8
```
`../../../` here allows us to move to the root directory, `..` implies parent in the linux file systems.


