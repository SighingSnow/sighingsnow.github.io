---
layout: post
title: "ReMesh With Geodesic"
categories: Graphics
---

### 1 Intro

[Polyscope](https://github.com/nmwsharp/polyscope) is used to visualize the data we got, which is what I also want to build one.

And this project is processed under instruction of Feichao Tian. Thanks a lot for his rich experience and patience.

### 2 Process

1. We get out geodesic process from this paper [Geodesic Distance Computation via Virtual Source Propagation](https://onlinelibrary.wiley.com/doi/full/10.1111/cgf.14371). It has O(N) linear time complexity. And what we appericate this paper is that, it's not so heavy and his way to construct indices of mesh is unusual.

   Because researchers won't always consider the non-manifold solution. We want to fix this problem in his code. But unfortunately, some T situations happen.

   And below is the picture which we map the geodesic data to a texture.

   ![](/assets/geodesic/contour.png)

2. We want to plug nodes on geodesic lines. First we get geodesic lines, and then plug nodes with a constant delta on it.  The delta is the plugging nodes' distance away from each other. And we can got this. The first picture is the geodesic lines we got. Blue lines are contour texture, and the yellow lines are geodesic lines.  And the second picture is the plugged points. While this time, geodesic lines is blue, and plugged point is yellow.

   ![](/assets/geodesic/line.png)

   ![](/assets/geodesic/plugged.png)

   

   

