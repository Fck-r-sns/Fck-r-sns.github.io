---
layout: post
year: 2016
title: "Dijkstra vs. A*"
subtitle: "Research on Dijkstra and A* pathfinding algorithms performance"
---

As my course project at the university's algorithms course, I decided to compare the performance of two pathfinding algorithms: Dijkstra's algorithm and A*. I was working on a libGDX game prototype called VT at that moment, so I decided it would be a good platform for tests. 

I created a navigation graph over a tiled level, implemented both algorithms there, and used some game UI elements to draw all the scanned nodes that algorithms visit before finding the optimal path. It was really helpful for algorithms understanding that Dijkstra's algorithm scans the navigation graph in all directions, and A* scans it in the direction of the expected target (if heuristics is ok).

![](/assets/img/personal-projects/dijkstra.png)
![](/assets/img/personal-projects/astar.png)

Technologies used: Java, libGDX.
 
Source code: [The course work at GitHub (in Russian)](https://github.com/binary-machinery/Dijkstra-vs-AStar), [The VT branch with the algorithms at GitHub](https://github.com/binary-machinery/VT/tree/pathfinding)
