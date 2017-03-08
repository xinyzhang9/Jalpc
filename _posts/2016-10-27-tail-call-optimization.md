---
layout: post
title:  "Tail call and optimization in ES6"
date:   2016-10-27
desc: "Tail call and optimization in ES6"
keywords: "JavaScript, ES6, recursion"
categories: [Javascript]
tags: [JavaScript,ES6]
icon: icon-javascript
---

As we know, if function calls himself, it is called recursion. However, recursion needs to take O(n) stacks in memory, which may cause stack overflow. For example,
