---
layout: post
title:  "Friend Circles(II)"
date:   2016-08-07
desc: "Friend Circles part 2"
keywords: "fullstack, algorithm, visualization, graph"
categories: [Javascript,Frontend]
tags: [FullStack,algorithm,visualization,graph]
icon: icon-javascript
---

## Forced-directed graph implementation
<image src = 'https://1.bp.blogspot.com/-yRWBJDIifCA/V6jQuPryfDI/AAAAAAAAIQw/PK6IcQ-Ftiw-LEEE_vI3ur5LGmGp3-RxQCPcB/s1600/screenshot3.png' width = '100%'></image>  

Force-directed layouts are so-called because they use simulations of physical forces to arrange elements on the screen.  It is very suitable to represent network data. In this project, I use a list of nodes and a list of edges to store graph data. We can store the nodes and edges inside one object.  

```
var dataset = {
        nodes: [
                { name: "Adam" },
                { name: "Bob" },
                { name: "Carrie" },
                { name: "Donovan" },
                { name: "Edward" },
                { name: "Felicity" },
                { name: "George" },
                { name: "Hannah" },
                { name: "Iris" },
                { name: "Jerry" }
        ],
        edges: [
                { source: 0, target: 1 },
                { source: 0, target: 2 },
                { source: 0, target: 3 },
                { source: 0, target: 4 },
                { source: 1, target: 5 },
                { source: 2, target: 5 },
                { source: 2, target: 5 },
                { source: 3, target: 4 },
                { source: 5, target: 8 },
                { source: 5, target: 9 },
                { source: 6, target: 7 },
                { source: 7, target: 8 },
                { source: 8, target: 9 }
        ]
};
```
There is one special requirements for d3.js to draw graph based on the nodes list and edges list. The source and target in edges must be represented in numbers. But for the data we just fetch from database, the source and target are represented as ObjectId(Strings). So we need do the ObjectId mapping.  

## ObjectID Mapping
The purpose of objectId mapping is to map a node ID(String) to a corresponding number(Integer). Also, we need to consider that the mapping is not static. If a user deletes his node, the mapped number should not be dead. To solve that problem, I came up with the idea that is to rebuild the mapping every time the controller fetch the updated data. The mapping function is like this:  

```
$scope.nodes = data;
$scope.obj = {};
var cnt = 0;
for(var key in data){
  if($scope.obj[data[key]._id] === undefined){
    $scope.obj[data[key]._id] = cnt;
    cnt++;
  }
}
```

After we build $scope.obj, we can rewrite the edges list:  

```
$scope.edges = data;
for(var key in $scope.edges){
  $scope.edges[key].source = $scope.obj[$scope.edges[key].source];
  $scope.edges[key].target = $scope.obj[$scope.edges[key].target];
}
```
The above mapping function guarantees the uniqueness and completeness of the datasets.  

## Draw graph in d3.js
After the d3.js library is imported, we can use the built-in function right inside the Angular controller. We define a draw function:  

```
var draw = function(){   
  //Width and height
  var w = 500;
  var h = 500;
  d3.select("svg").remove();
  var svg = d3.select("#display")
       .append("svg")
       .attr("width",w)
       .attr("height",h);

  var colors = d3.scale.category10();
  var dataset = {
           nodes: $scope.nodes,
           edges: $scope.edges
  };


  var force = d3.layout.force()
                     .nodes(dataset.nodes)
                     .links(dataset.edges)
                     .size([w, h])
                     .start()
                     .linkDistance([50])       
                     .charge([-100])            
                     .start();
  var edges = svg.selectAll("line")
                 .data(dataset.edges)
   .enter()
                 .append("line")
   .style("stroke", "#ccc")
   .style("stroke-width", 1);
  var nodes = svg.selectAll("circle")
   .data(dataset.nodes)
                 .enter()
   .append("circle")
   .attr("r", 10)
   .style("fill", function(d, i) {
                   return colors(i);
          })
   .call(force.drag)
          .on('mouseover', tip.show)
   .on('mouseout', tip.hide)
   .on('dblclick', connectedNodes); //Added 
  force.on("tick", function() {
    edges.attr("x1", function(d) { return d.source.x; })
  .attr("y1", function(d) { return d.source.y; })
         .attr("x2", function(d) { return d.target.x; })
  .attr("y2", function(d) { return d.target.y; });
    nodes.attr("cx", function(d) { return d.x; })
  .attr("cy", function(d) { return d.y; });
  });
  
}//draw
```
The basic idea is after we got $scope.dataset ready, we create a SVG and start putting everything on the panel. The elements have unique css classes. Using the event-driven model, we can manipulate the elements when receiving the action of user, such as 'mouseover' or 'double click'.
We also make use of callback functions to change status. For example, in the code  

```
.on('dblclick', connectedNodes)   
```

ConnectedNodes is the callback function when the event 'double click' is triggered.  

Based on the basic structure, more complicated features such as "Degree of separation" can be implemented using some graph algorithms. It will be introduced in the next post. Thanks for reading!  

## Link to related blog
[Part III](https://xinyzhang9.github.io/home/javascript/frontend/2016/08/08/friend-circle.html)
