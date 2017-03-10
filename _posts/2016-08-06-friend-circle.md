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
I used the classical graph structure to design the user schema.  

The NodeSchema saves the information of the user. To make things easier, I use the self-join techniques to make the "friends" attributes, so it can keep track the current user's friends.  

```
var NodeSchema = new mongoose.Schema({
  name:String,
  gender: {type:String, default:"f"},
  created_at:Date,
  friends: [{
    type: mongoose.Schema.Types.ObjectId, ref: 'nodes'
  }],
});

```
The EdgeSchema saves the relationship between users. In this project, there is no actual directions. If two users are connected, there is two-way connection. "source" and "target" just follows the name convention.  

```
var EdgeSchema = new mongoose.Schema({
  source : { type: Schema.ObjectId },
  target : { type: Schema.ObjectId },
  created_at : Date,
});
```

## Server-side controller design
The server-side controller talks to the database directly and parse data back to front-end. The typical code block is like this:  

```
module.exports = (function(){
return{
         index:function(req,res){
             Node.find({},function(err,output){
               if(err){
                 console.log(err);
               }else{
                 res.json(output);
               }
             })
         },

......
       }
})()
```

I used two server-side controllers here. One for node and one for links(edges).  

## Routes design
I followed the RESTful pattern to design the routes.  For example, :/id represents the node objectId parsed as a parameter.  

```
app.get('/nodes/:id',function(req,res){
  nodes.getNodeById(req,res);
});
```

## Angular factory design
The factory serves the communication between backend and front-end controllers. It can also be regarded as the data provider to the front-end. The main ejection to the factory module is the "http" service. It makes a http request to the router and get data back to the controller. A typical code block is like this:  

```
myApp.factory('nodeFactory',function($http){
   factory = {};
   var nodes = [];
   factory.index = function(callback){
    console.log('nodeFactory is loading data');
    $http.get('/nodes').success(function(output){
     console.log(output);
     callback(output);
    })
   };
......
   return factory;
  });
```

## Angular controller design
The controller controls all the data exchange in the html pages. The scope variables are not only used to render corresponding data on the page, they are also used as state indicator. Since the scope variable lives inside the controller, it can be shared by multiple functions in the controller.  

The hardest point in this project is to make sure before the drawing happens, the graph data are ready. After some trials and optimization. I used some callback chains to get the things done. For example, to create connections, there are three steps:  
* Add user2 to user1's friend list
* Add user1 to user2's friend list
* Add links between user1 and user2  

```

$scope.connect = function(id2){
nodeFactory.connect(id,id2,function(data){
  nodeFactory.connect(id2,id,function(data){
    edgeFactory.create(id,id2,function(data){
      getNode(id);
      getNodes();
     // getEdges();
     // draw();
    })
  })
})
```

For the key function, that is drawing graph via d3.js, I will create another post to illustrate it. Hope this post gives you the overview of the architecture of this project. Thanks for reading!  

## Link to related blog
[Part II](https://xinyzhang9.github.io/home/javascript/frontend/2016/08/07/friend-circle.html)
