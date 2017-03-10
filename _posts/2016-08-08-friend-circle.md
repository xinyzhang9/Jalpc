---
layout: post
title:  "Friend Circles(III)"
date:   2016-08-08
desc: "Friend Circles part 3"
keywords: "fullstack, algorithm, visualization, graph"
categories: [Javascript,Frontend]
tags: [FullStack,algorithm,visualization,graph]
icon: icon-javascript
---

## Prepare for the graph structure
<image src = 'https://4.bp.blogspot.com/-7JdDK_3zMS0/V6jQuKkpS8I/AAAAAAAAIQw/W_wJ5LwHsfUOlmCVTJypBbBh2F8aEKCFACPcB/s1600/screenshot4.png' width = '100%'></image>    

I will implement the degree of separations between two nodes. The simple data structure(node list + edge list) is not capable. We need to build a graph object in order to process the graph algorithms. The graph builder is defined like this:  
```
function Graph() {     
  var neighbors = this.neighbors = {}; // Key = vertex, value = array of neighbors.
  this.addEdge = function (u, v) {
    if (neighbors[u] === undefined) {  // Add the edge u -> v.
      neighbors[u] = [];
    }
    neighbors[u].push(v);
    if (neighbors[v] === undefined) {  // Also add the edge v -> u in order
      neighbors[v] = [];               // to implement an undirected graph.
    }                                  // For a directed graph, delete
    neighbors[v].push(u);              // these four lines.
   };
   return this;
}
```

And the prepareGraph function is defined like this:  

```
function prepareGraph(edges){    
  var graph = new Graph();
  for(var i = 0; i < edges.length; i++){
    graph.addEdge(edges[i].source,edges[i].target);
  }
  console.log('prepare graph',graph);
  return graph;
}
```

## Shortest path
Based on the graph object, I wrote the function to calculate shortest path between two nodes based on ***breadth-first-search*** algorithms.

```
function shortestPath(graph, source, target) {     
  if (source == target) {   // Delete these four lines if
    print(source);          // you want to look for a cycle
  return;                 // when the source is equal to
  }                         // the target.
  var queue = [ source ],
  visited = { source: true },
  predecessor = {},
  tail = 0;
  while (tail < queue.length) {
    var u = queue[tail++],  // Pop a vertex off the queue.
    neighbors = graph.neighbors[u];
    for (var i = 0; i < neighbors.length; ++i) {
      var v = neighbors[i];
      if (visited[v]) {
        continue;
      }
      visited[v] = true;
      if (v === target) {   // Check if the path is complete.
        var path = [ v ];   // If so, backtrack through the path.
        while (u !== source) {
          path.push(u);
          u = predecessor[u];
        }
        path.push(u);
        path.reverse();


        print("The Degree of Separation to the target is "+path.length+'.');

        return path;
      }

      predecessor[v] = u;
      queue.push(v);
    }
  }
  print('There is no path to the target');
  return null;
}
```
The result is called "Degree of Separations". There is a famous theory in social networks that is any two people can be connected through no more than 6 people. This is called "Six Degree of Separations". I think if the data is big, I can use this project to prove that theory.  

## Draw animation
I think the most difficult part of this project is to draw the animation from the start node to the target node following the calculated shortest path.  I spent hours to debug. The bug is the path, for example [1,4,2,3] is not in the right order in the actual graph representation. Finally, I checked the source code using Chrome inspect tool, and wrote a sorting function.  

```
//sort in order of original arr!     
selected[0].sort(function(a,b){
  return d3.ascending(arr.indexOf($scope.obj[a.__data__._id]),arr.indexOf($scope.obj[b.__data__._id]))
});
```

After sort, the node representation in the path matches the actual nodes in graph. The drawing code is like this.  

```
selected.transition()        
  .duration(500)
  .delay(function(d, i) { return i * 1000; })
  .style("opacity","1");
link.transition()
  .duration(500)
  .style("opacity", function (o) {
    return arr.indexOf($scope.obj[o.source._id]) <0  || arr.indexOf($scope.obj[o.target._id]) <0 ? 0.1 : 1;
  });
}
```

## It is not the end
Due to other work in my schedule, I just keep the project in beta version. There are many more fun features to be added. I also plan to crawl the real data from social networks website, such as twitter and facebook to build a real, huge neural networks. Thanks for reading!  
## Link to related blog
[Part IV](https://xinyzhang9.github.io/home/javascript/backend/2016/08/09/friend-circle.html)
