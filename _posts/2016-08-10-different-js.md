---
layout: post
title:  "Difference in clientJS and serverJS"
date:   2016-08-10
desc: "Difference in clientJS and serverJS"
keywords: "JavaScript, client, server"
categories: [Javascript,Frontend,Backend]
tags: [JavaScript, client, server]
icon: icon-javascript
---
Recently I started to drive deep into NodeJS. Compared with the JavaScript run on the browser, the JavaScript run on the server has many difference.

* JS inside browser is distributed by the same server and is executed in multiple client endpoints; JS on server side is executed multiple-times just on the server.  
* JS inside browser's bottleneck is the bandwidth of internet; JS on server side's bottleneck is the system resources like CPU and memory.  
* JS inside browser is loaded from internet; JS on server side is loaded from hard disk. The loading speed differs in order of magnitude.  


In summary, although javascript is in used in the full-stack, it doesn't mean it governs everything without trouble. Works must be done to balance the front end and back end.
