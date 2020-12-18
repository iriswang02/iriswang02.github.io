# Video Coding

 

> Introduction 



Digital video communication can be found today in many application scenarios.

What is the basic problem of communication?

- how to convey source data with the **highest fidelity** possible within an **available bit rate**?
- or how to convey the source data using the **lowest bit rate** possible while maintaining a **specified reproduction fidelity**?

The ability of a source coding system to make this tradeoff between bitrate and fidelity well is called **coding efficiency** or **rate-distortion performance**, and the coding system itself is referred to as a **codec**.

Thus, video codecs are primarily characterized in terms of :

- Throughput of the channel
- Distortion of the decoded video

However, in practical video transmission systems the following additional issues must be considered as well.

- Delay (startup latency and end-to-end delay)
- Complexity (memory capacity and memory access requirements)

Hence, the practical source coding design problem is posed as follows:

***given a maximum allowed delay and a maximum allowed complexity, achieve an optimal tradeoff between bitrate and distortion for the range of network environments envisioned in the scope of applications.***



Some international video standards:

- H.264 - Advanced Video Coding(AVC)

- H.265 - High Efficiency Video Coding(HEVC)

- H.266 - Versatile Video Coding(VVC)