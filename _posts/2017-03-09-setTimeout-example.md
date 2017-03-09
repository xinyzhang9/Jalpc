---
layout: post
title:  "Use setTimeout to reorder function call"
date:   2017-03-09
desc: "Use setTimeout to reorder function call"
keywords: "JavaScript, setTimeout"
categories: [Javascript]
tags: [JavaScript,setTimeout]
icon: icon-javascript
---
## prior knowledge
***setTimeout*** is a common function to execute a function after a fixted time.
```
setTimeout(function, millsec, args)
```
## 0 as parameter
```
setTimeout(function(){
  ...
},0)
```
What is the meaning of the avove code block? Actually 0 makes sense. It changes the order of task execution of the browser. The browser will not execute the function in setTimeout until it resolves current task queue.

## example
write a html file like this
```
<html>
<head>
	<title>setTimeout example</title>
</head>
<body>
	<p id = 'p1'>
		<input type = 'text' id = 'i1' value = ''>
		<span id = 's1'></span>
	</p>
	<p id = 'p2'>
		<input type = 'text' id = 'i2' value = ''>
		<span id = 's2'></span>
	</p>
	<script type="text/javascript">
		document.getElementById('i1').onkeydown = function(){
			document.getElementById('s1').innerHTML = this.value;
		}
		document.getElementById('i2').onkeydown = function(){
			setTimeout(function(){
				document.getElementById('s2').innerHTML = document.getElementById('i2').value;
			},0)
		}
	</script>
</body>
</html>
```
![alt](https://github.com/xinyzhang9/frontend_must_knows/blob/master/setTimeOut/setTimeout.png?raw=true)
The first line is the result of onkeydown event without using setTimeout. You can see the result is missing the last input from the user, since the execution order is onkeydown -> onkeypress -> onkeyup  

The second line is the result using setTimeout. It updates the value before extracting it to the right span. The execution order is onkeydown -> onkeypress -> function -> onkeyup  
