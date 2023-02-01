---
layout: post
toc: true
title: "Convert ffmpeg yuv420p frame to QVideoframe"
categories: Multimedia
tags: [Multimedia] [ffmpeg] [qt]
author: Tingyu Song 
---

The way to convert avframe to qvideoframe under the qt newest version(6.2).

### 1 Questions

I have seen several ways to convert a decoded avframe to a QVideoframe by this way below.

```c++
		QVideoFrame frame(size, QSize(_width, _height), _width, this->_format.pixelFormat());
    if (frame.map(QAbstractVideoBuffer::WriteOnly)) {
      memcpy(frame.bits(), buffer->DataY(), size);
      frame.setStartTime(0);
      frame.unmap();
      Q_EMIT this->newFrameAvailable(frame);
    }
```

But under the newest version of qt(6.2) , the `frame.bits()` can no longer be used. When using `frame.bits()` you should specify which plane do you want to copy. Thus I used the code below and fails. I think as avframe has `data[0] data[1] data[2]` and `QVideoframe` has 3 plane, I can just copy the data from avframe to QVideoframe.

```c
memcpy(qframe.bits(0),frame_->data[0],w*h);
memcpy(qframe.bits(1),frame_->data[1],w*h>>2);
memcpy(qframe.bits(2),frame_->data[2],w*h>>2);
```

And I got the QVideoframe presents so weird.

![](/assets/yuv420p2qvideoframe/1.png)

### 2 Answers

You can use the code below to copy a yuv420 format avframe to QVideoframe by the api provided by ffmpeg.

```c++
av_image_copy_plane(qframe.bits(0),qframe.bytesPerLine(0),frame_->data[0],frame_->linesize[0],w,h);
av_image_copy_plane(qframe.bits(1),qframe.bytesPerLine(1),frame_->data[1],frame_->linesize[1],w/2,h/2);
av_image_copy_plane(qframe.bits(2),qframe.bytesPerLine(2),frame_->data[2],frame_->linesize[2],w/2,h/2);
```

And the picture shown will get better.

![](/assets/yuv420p2qvideoframe/2.png)
