---
layout: post
toc: true
title: "ReMesh With Geodesic"
categories: Graphics
tags: [remesh, geodesic, delaunay]
comments : true
author:
   - Feichao Tian
   - Tingyu Song
---

### 1 Intro

[Polyscope](https://github.com/nmwsharp/polyscope) is used to visualize the data we got, which is what I also want to build one.

And this project is processed under instruction of Feichao Tian. Thanks a lot for his rich experience and patience.

### 2 Process

1. We get out geodesic process from this paper [Geodesic Distance Computation via Virtual Source Propagation](https://onlinelibrary.wiley.com/doi/full/10.1111/cgf.14371). It has O(N) linear time complexity. And what we appericate this paper is that, it's not so heavy and his way to construct indices of mesh is unusual.

   Because researchers won't always consider the non-manifold solution. We want to fix this problem in his code. But unfortunately, some T situations happen.

   And below is the picture which we map the geodesic data to a texture.

   ![Fig. 1](/assets/geodesic/contour.png)

   <center>Fig. 1. countour result</center>

2. We want to plug nodes on geodesic lines. First we get geodesic lines, and then plug nodes with a constant delta on it.  The delta is the plugging nodes' distance away from each other. And we can got this. The first picture is the geodesic lines we got. Blue lines are contour texture, and the yellow lines are geodesic lines.  And the second picture is the plugged points. While this time, geodesic lines is blue, and plugged point is yellow.

   ![Fig. 2](/assets/geodesic/line.png)

   <center>Fig. 2. geodesic line visualization</center>

   ![Fig. 3](/assets/geodesic/plugged.png)

   <center>Fig. 3. inserted nodes visualization</center>

3. Third, we generate deluanay triangle from these plug nodes. We get those plug nodes and their faceID. And sort them by their faceid. After sorted, we deluanay triangulate the mesh triangle by triangle. Below is this step's result.

   ![](/assets/geodesic/deluanay.png)

   <center>Fig. 4. deluanay triangulation</center>

4. Fourth, we need some flip operations and merge operations to do this. In this step, we use pmp-library as our tools. We use it to do flip and merge operations. 

   ![](/assets/geodesic/flip&merge.png)

   <center> Fig. 5. flip & merge operations </center>

   First we do clip until there is no edges to flip or only few edges to flip. Then we do merge, merge the origin input mesh's vertex to the inserted nodes.

   ![](/assets/geodesic/merge_res.png)

   <center>Fig. 6.  Final result</center>

### 3 Result

We've tested several models. And the program robusty is guaranteed.

Below are results for complicate models.

.![](/assets/geodesic/armadillo.png)

<center>Fig. 7. armadillo origin</center>

![](/assets/geodesic/armadillo_remesh.png)

<center>Fig. 8. remesh armadillo</center>

![](/assets/geodesic/beetle.png)

<center>Fig. 9. beetle origin</center>

![](/assets/geodesic/beetle_remesh.png)

<center>Fig. 10. remesh beetle</center>
