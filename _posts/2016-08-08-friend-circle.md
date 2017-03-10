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
<image src = 'https://4.bp.blogspot.com/-7JdDK_3zMS0/V6jQuKkpS8I/AAAAAAAAIQw/W_wJ5LwHsfUOlmCVTJypBbBh2F8aEKCFACPcB/s640/screenshot4.png' width = '100%'>

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
