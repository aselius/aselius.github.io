---
layout: post
title: Maze Solving, Path Finding, and Satellite Navigation - Part 2-
date: 2017-12-03 21:23:15 -0800
description: This post deals with explaining the introduction to path planning, maze solving, and potentially how satellite navigation works. Part 2 of a 3 part series, focuses on explaining the A * algorithm and extends into explaining how it can be used in a maze solver type of situation.
img: path_planning/maze.jpg
tags: [Tech, Graph Theory, Path Planning, Self Driving Cars, A*, Priority Queue]
---
In part 1, I briefly talked about what path planning is, and how Dijkstra's algorithm can be used to tackle such a problem. So picking up where we left off, I will start by recapping Dijkstra's and point out the short comings, and why it's not applicable for every occasion.

## Recapping Dijstra's Algorithm

This is still work in progress. I lost all the work I had written previously, so I need to do a rewrite with some python examples and using a non static element graph hopefully haha. I'll do a rewrite before 2019 ends :)

## Problems with Dijkstra's

![Dense Network]({{site.baseurl}}/assets/img/path_planning/dense_network.png)

## Adding a heuristic to the picture

![Path Network with Heuristic]({{site.baseurl}}/assets/img/path_planning/path_network_with_heur.png)

