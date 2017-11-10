---
layout: post
title: Maze Solving, Path Finding, and Satellite Navigation - Part 1-
date: 2017-11-09 22:53:20 -0800
description: This post deals with explaining the introduction to path planning, maze solving, and potentially how satellite navigation works. Part 1 of a 2 part series, focuses on explaining what you need to consider in path planning, the Dijkstra Algorithm, and the problem that arises with Dijkstra.
img: path_planning/neural_net.jpg # Add image post (optional)
tags: [Tech, Graph Theory, Path Planning, Self Driving Cars]
---
>I'll color code the tables when I have time.. apologies for the visibility on the tables. (And spell check things..)

If you talk to me every now and then, you would probably know that my latest craze is about self driving cars. I was extremely excited when Udacity announced the Self Driving Car Nanodegree back around this time of the year last year. I signed up immediately and was due to start in December, and have been progressing through it albeit a little slower than I would have liked. I had to delay Term 2 and Term 3 by a month each because of just so much going on around me. I am really looking forward to going back and reviewing most of the materials I dealt with, and writing about it, but that will come in a later time. But anywho, this is the final stretch! Last term, and it begins with path finding.

## What is Path Finding?

Pathfinding is related to the shortest path problem in graph theory, and it boils down into finding the shortest route between two nodes. It is used in various realms of computer science, with one being (an interesting application I thought), finding out what the most optimal path is in a network to limit latency, and another being satelite navigation. 

>### How does Satellite Navigation work?
>An aside on satellite navigation. Satellites are in essence a timer circulating around the orbit, which broadcasts time 24/7. Since the satellites broadcast time in radio waves, when you receive the signal, the receiver can figure out how far away it is from that satellite. So with this, by getting multiple satellites to broadcast the time, we can look at where they intersect to get an understanding of where we currently are in terms of our position on earth. I digress. Back to Pathfinding.

## So How Do We Implement Path Finding?

There are two sides to the path finding problem. The first being, finding where you are, and the second being getting from the beginning location to the destination location. So say you have a rough understanding of where you are using the GPS location. Now, how do you get from the start point to the end point? We should look at a couple of algorithms that can help us achieve that cause.

The most common method of solving this problem is with Dijkstra’s algorithm, and we will start off with this. If you want to skip Dijkstra and go straight into A*, go [here](#).

With that, lets take a look at a graph.

![Path Network]({{site.baseurl}}/assets/img/path_planning/path_network.png)

Let us take the above graph topology for example. The nodes are connected to each other by routes, and each route has its own step size if you will. It doesn’t necessarily mean how far away each node is. It just might be that there is a slow down between the two routes. So think of the numbers as just that: step sizes (what the cost is to go from node A to node B).

We are starting at Node S (Start), and want to get to Node E (End). How would we address this problem just on plain sight? We can use our human intuition for this example, since its very simple. Obviously there is an express way down S-B-H-G-E. If we want the computer to perform such an action without any prior knowledge about Graph Theory, we could probably also use a brute force method and traverse through every combination of Nodes to figure out which way is fastest. Well, it should be easy enough to solve this example with brute force, but if the network grows and becomes more complex, the compute time for such an algorithm would be horrendous. We need a better, faster method that also ensures that the route we find is in fact the shortest route, and we didn’t miss one just out of shortsightedness.

## Dijkstra’s Algorithm

Dijkstra is similar to a brute force method, but is guided somewhat with a priority queue. It makes use of an order that makes most sense based on how costly a path is. Let’s bring out a table to illustrate how.

| Node  | Cost   | Goes Through |
|------ |:------:|--------------|
| S     |   0    |  S |
| A     |   Inf  | - |
| B     |   Inf  | - |
| ...   |   ...  | - |

So we start off by listing all the nodes in the table. The starting node S gets a 0 cost (or distance) since it is the starting point, all the while, the other nodes will get a cost of infinity since we have not reached that node yet and it is uncertain. So with this we look at what connections S has. We also take note of where the last node it traversed through is. This is important in updating the cost should another path have a lower cost to that specific node.

![Path From S]({{site.baseurl}}/assets/img/path_planning/path_from_s.png)

So we can randomly choose one. For convenience’s sake, let us start alphabetically with A. S-A has a cost of 7, and this is lower than the Infinite cost we have assigned. So we update the table. Likewise, for the rest of the connections S has, (B and C), the table can be updated as following.

To put the first pathway S-A on the priority queue,

| Node| Cost| Goes Through |
|-----|-----|--------------|
|  S  |  0  |  S           |
|  A  |  7  |  S           |
|  B  | Inf |  -           |
| ... | ... | ...          |

Putting all the pathways S-A, S-B, S-C in the priority queue,

| Node| Cost| Goes Through |
|-----|-----|--------------|
|  S  |  0  |    S         |
|  B  |  2  |    S         |
|  C  |  3  |    S         |
|  A  |  7  |    S         |
|  D  | Inf |    -         |
| ... | ... |   ...        |

You can notice that this priority queue organizes the rows by the lowest cost. Now that we’ve exhausted all the connections S has, we can go ahead and pop S out of the queue. After which we pick the next lowest cost Node, which in this case is B.

![Path From B]({{site.baseurl}}/assets/img/path_planning/path_from_b.png)

The connections B has is as follows. But since we are just coming from S, we ignore that route back to S. We are left with A, D, H. Retracing what we did for the nodes connected to S, we get the following.

Putting the pathway B-A in the priority queue,

| Node| Cost| Goes through |
|-----|-----|--------------|
|  B  |  2  |  S  |
|  C  |  3  |  S  |
|  A  | 2+3 |  B  |
|  D  | Inf |  -  |
| ... | ... | ... |

For B-D pathway,

| Node| Cost| Goes through |
|-----|-----|--------------|
|  B  |  2  |  S  |
|  C  |  3  |  S  |
|  A  |  5  |  B  |
|  D  | 4+2 |  B  |
|  H  | Inf |  -  |
| ... | ... | ... |

For B-H pathway,

| Node| Cost| Goes through |
|-----|-----|--------------|
|  B  |  2  |  S  |
|  C  |  3  |  S  |
|  A  |  5  |  B  |
|  D  |  6  |  B  |
|  H  | 2+1 |  B  |
| ... | ... | ... |

Notice the B-A connection. Since the cost of getting to A from the start point is higher at 7 than going through B, we can update A with a lower cost of 5 (3 from B + 2 to B from S). We proceed with the other nodes connected to B, and we pop B out since there aren’t any connections left. Oof, now let’s speed this up. 

The next up is H or C. At first sight, it seems like it will take less time if it picks H since, the only node H is connected to is G, which is only connected to E right? Not quite. Remember how we are choosing the next row up in the priority queue? Since the shortest distance is 7 (yes, let’s cheat a little), you will have to exhaust all the other nodes that have a value under 7 in order for the algorithm to finish. Recall that we want to confirm that the path we have chosen is actually the shortest path, and we didn’t miss a path that was shorter. You can only confirm this if E rises to the top of the priority queue. If it is down below, the algorithm persists. So the first time you see E with a cost that is not infinite, the table will look something like this.

| Node| Cost| Goes through | Popped? |
|-----|-----|--------------|---------|
|  B  |  2  |  S  |  Yes  |
|  C  |  3  |  S  |  Yes  |
|  H  |  3  |  B  |  Yes  |
|  A  |  5  |  B  |  Yes  |
|  L  |  5  |  C  |  Yes  |
|  G  |  5  |  H  |  Yes  |
|  D  |  6  |  B  |   No  |
|  F  |  6  |  H  |   No  |
|  E  |  7  |  G  |   No  |
|  I  |  9  |  L  |   No  |
|  J  |  9  |  L  |   No  |

Ah, we have arrived at E, but we still have to go through D and F, and proceed to the connections linked to D and F before E rises to the top of the queue.

## What have we learned?

So now you know how Dijkstra works. Imagine if this network is giant, with connections sprawling everywhere. You would prioritize shorter pathways since they rise to the top of the queue faster. However, there is a problem (as you might have noticed). Dijkstra doesn’t take direction into perspective. Although we knew that by going to node H, we were getting closer to node E, we still proceeded to look at C and traverse down. So Dijkstra will go down a pathway that is shortest, and only back track and go the right direction when that pathway you were going down no longer becomes the shortest path, and the actual pathway leading to the goal becomes the “shortest” path.

A* algorithm solves this problem pretty nicely, but since this post is pretty lengthy, I will postpone that discussion for part 2. Cheers :)


