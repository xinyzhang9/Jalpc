---
layout: post
title:  "Event Delegaton in JavaScript"
date:   2016-08-17
desc: "Event Delegaton in JavaScript"
keywords: "JavaScript, event"
categories: [Javascript,Frontend]
tags: [Javascript]
icon: icon-javascript
---
Many developers may hear about event delegation but not familiar with that. I just wrote and tested some code to illustrate how it works. Event delegation is to add a event listener to the parent instead of the individual node. In this way we can avoid redundant codes for similar listeners.  
### Example 1   
```
<head>
 <title>delegation</title>

</head>
<body>
 <ul id = "parent">
  <li id = "l1"> Item 1 </li>
  <li id = "l2"> Item 2 </li>
  <li id = "l3"> Item 3 </li>
  <li id = "l4"> Item 4 </li>
  <li id = "l5"> Item 5 </li>
 </ul>
 <script type="text/javascript">
  document.getElementById("parent").addEventListener("click",function(e){
   if(e.target && e.target.nodeName == 'LI'){
    console.log("List Item", e.target.id.slice(1),"was clicked!");
   }
  })
 </script>
</body>
```
The test result is as shown below.  
![alt](https://2.bp.blogspot.com/-VxwcUJwn6A0/V7TGOvjWXNI/AAAAAAAAIbM/bRnWWKZlFjAT21E657w2avBJeZHVfY0KwCLcB/s320/Screen%2BShot%2B2016-08-17%2Bat%2B1.14.30%2BPM.png)  

### Example 2
```
<div id = "div2">  
  <p>Test delegation matching.</p>
  <a href="#" class = "classA">link1</a>
  <a href="#" class = "classB">link2</a>
</div>

<script>
  document.getElementById("div2").addEventListener("click",function(e){
    if (e.target && e.target.matches('a.classA')) {
      console.log("Anchor was clicked!")
    };
  })
</script>
```
The test result:  
![alt](https://3.bp.blogspot.com/-wggXclOwHaw/V7THxP2cYQI/AAAAAAAAIbc/P1PsyZP3-n4oDNGk6nnt5I-AI_22M-ZMACLcB/s320/Screen%2BShot%2B2016-08-17%2Bat%2B1.22.33%2BPM.png)  
In summary, event delegation can let parent node perform events on children nodes. It saves lots of event listeners, also makes code simpler and more elegant.  

[Reference](https://davidwalsh.name/event-delegate)




