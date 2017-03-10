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

<image src = 'https://4.bp.blogspot.com/-7JdDK_3zMS0/V6jQuKkpS8I/AAAAAAAAIQw/H_79SGzaVXcqbn59xpuktb_NKbewjdhjACEw/s1600/screenshot4.png'
width = '100%'></image>  

## Architecture
<image src = 'https://2.bp.blogspot.com/-k6lPdlyJgYo/V6jQzpMcsBI/AAAAAAAAIQo/AN8IX75VBwA2ohKanesvuQPA-r9pAq2NACLcB/s1600/friend-circles.002.jpeg' width = '100%'></image>  

* AngularJS is one of the most popular MVC frameworks. I used it to build my front-end with the classical flow. View <=> Controller <=> Factory <=> Routers <=> Backend. I also utilized its built-in validations and filters. For example, a user can only disconnect to another user if they have already connected.
* D3.JS is the javascript library which brings data to live using HTML, SVG and CSS. The main function is to draw the real-time networks based on the fetched data from backend. The syntax and programming logic is almost same as the pure javascript. I embedded the code inside my Angular controller. So it can make use of the scope variables and keep updating fluently.
* Express is a minimal and flexible NodeJS web framework. It has a lot of APIs and built-in features to make the application easier to build.
* NodeJS is the server-side javascript framework. It handles I/O in asynchronized way and it will not block the main flow of the application. The event-driven model is very suitable to handle large quantities of queries at the same time.
* MongoDB is one of the most popular Non-SQL databases. It has rich query language and high performance and scalability. In this project, there is no complicated queries so the MongoDB is the best choice.  

## Schema design

