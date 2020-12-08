# Structure Analysis

 

> Introduction 

1. AVFrame: This structure describes decoded (raw) audio or video data.

   - AVFrame is typically allocated (`av_frame_alloc()`) once and then reused multiple times to hold different data (e.g. a single AVFrame to hold frames received from a decoder). 

   - The data described by an AVFrame is usually reference counted through the AVBuffer API.

   - sizeof(AVFrame) is not a part of the public ABI, so new fields may be added to the end with a minor bump.

2. AVFrameContext

3. AVCondecContext

4. AVIOContext

5. AVCodec

6. AVStream

7. AVPacket



 ![structure](assets\structure.jfif) 

(Image source: https://blog.csdn.net/leixiaohua1020/article/details/11693997)