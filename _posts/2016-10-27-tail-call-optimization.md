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
```
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}
factorial(5) // 120
```

If the recursion is the last step of the function, it is called tail call. It only takes O(1) stack in memory, so it will not cause stack overflow. The factorial function can be rewritten like this:  
```
function factorial(n,total=1){
 if(n <= 1) return total;
 return factorial(n-1,total * n);
}


factorial(5) // 120
```
If a function is written in ES6 'use strict' format, it will be optimized automatically.
For the optimization detail, please refer to the ES6 docs

For further reading, please see this [Reference](http://es6.ruanyifeng.com/#docs/function)


