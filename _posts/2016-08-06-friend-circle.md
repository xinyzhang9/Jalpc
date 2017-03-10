---
layout: post
title:  "Friend Circles(I)"
date:   2016-08-06
desc: "Friend Circles part 1"
keywords: "fullstack, visualization, architecture"
categories: [Javascript,Frontend,Backend]
tags: [FullStack,visualization,architecture]
icon: icon-javascript
---
Inspired by the neural networks, recently did a project called friend-circles. It draws the real-time social networks between all registered users in my website. The initial purpose is I want to see how powerful and exciting the data visualization techniques could bring to life.  

## Live
[live demo](http://104.236.188.141/#/)
## Github page
[source code](https://github.com/xinyzhang9/friend-circles)

## Overview
Index page  
<image src = 'https://2.bp.blogspot.com/-CyLsDCmpVrc/V6jQq3703oI/AAAAAAAAIQc/rx45UA_WPn4t3f7hl9vLrLL0y0aau6HUgCLcB/s1600/screenshot5.png' width = '100%'><image>  
User page  
<image src = 'https://2.bp.blogspot.com/-yRWBJDIifCA/V6jQuPryfDI/AAAAAAAAIQk/2u3BjkU-9wwq7wLGEa9xX94wq9RKXAW6wCLcB/s1600/screenshot3.png' width = '100%'><image>  

On the left side is the control panel, user can select "connect" or "disconnect" on any of other users.
The middle panel displays the real-time networks of all the users. Each user is represented as a distinct node. Links simply mean they have connections between each other.  

On the right side is the ranking panel, which ranks the top users with most connections.  

I also added some advanced features based on this.  

* Tooltip: When the mouse hover on the node, the corresponding user name is shown.
* Highlight: When a node is double clicked, the direct connections will be highlighted and others will be set to almost invisible.
* Search & Find me: User can search for a name, the corresponding node will be located. Other nodes will be set to invisible for seconds. If the searched name is empty, it will locate the user himself.
* Degree of separation: User can search for another user, it will draw the shortest path between the two nodes, showing the degree of separation in animation. in other words, it shows how to connect the target person through your existing connections. (shown below)
