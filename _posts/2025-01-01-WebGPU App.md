---
layout: post
toc: true
title: "A quick start of WebGPU"
categories: Graphics
comments: true
tags: [web, webgpu]
author: 
  - Tingyu Song 
---

I followed the [your-first-webgpu-app](https://codelabs.developers.google.com/your-first-webgpu-app#0.) and finished the course in my spare time. 

And the result is as follows:
![](/assets/webgpu/result.gif)

The code is quite straightforward, but why I wrote this blog is that I was inspired by the course. 

So, the takeaways are as follows:

* A compute shader is an effective way to leverage the GPU for calculations. Sometimes, we don’t have ready-made solutions for certain scenarios. Reflecting on this reminds me of when I used the CPU for similar calculations in the past. 

* Ping-pong buffer usage: I have occasionally experimented with this type of buffer in my projects. In the past, I relied on approaches involving mutexes or semaphores to manage the pipeline. However, knowing about this buffer and its name earlier could have saved me considerable time. With the proper terminology, I could have directly searched for and adopted useful design patterns instead of reinventing the wheel in a naive way.