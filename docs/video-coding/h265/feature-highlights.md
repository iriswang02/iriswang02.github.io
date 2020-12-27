# Feature Highlights

GOALS:

- coding efficiency
- ease of transport system integration
- data loss resilience
- parallel processing architectures

### Video Coding Layer

The video coding layer of HEVC employs the same hybrid approach (inter-/intra-picture prediction and 2-D transform coding) used in all video compression standards since h.261.

- Coding tree units (CTU) and coding tree block (CTB) structure:

  **Macroblock (h.264) -> CTU (h.265)**

   CTU consists of  a luma CTB and the corresponding chroma CTBs and syntax elements. 

  The size of CTU is selected by the encoder. <u>The size LxL of a luma CTB can be chosen as L=16, 32, or 64 samples</u>, with the larger sizes typically enabling better compression. 

  HEVC then supports a partitioning of the CTBs into smaller blocks using a tree structure and quadtree-like signaling.

- Coding units (CUs) and coding blocks (CBs):

  The quadtree syntax of the CTU specifies the size and positions of its luma and chroma CBs. <u>The root of the quadtree is associated with the CTU.</u>

  One luma CB and ordinarily two chroma CBs, together with associated syntax, form a coding unit (CU). 

  A CTB (CTU?) may contain only one CU or may be split to form multiple CUs, and each CU has an associated partitioning into prediction units (PUs) and a tree of transform units (TUs).

  ```
  CTU (consists of a luma CTB and the corresponding chroma CTBs and syntax elements)
   |
   V
  CUs (consists of a luma CB and two chroma CBs, together with associated syntax)
   |
   V
  PUs & TUs
  ```

- Prediction units and prediction blocks:

  The decision whether to code a picture area using inter-picture or intra-picture is made at the CU level. <u>A PU partitioning structure has its root at the CU level.</u>

  Depending on the basic prediction-type decision, the luma and chroma CBs can then be further split in size and predicted from luma and chroma prediction blocks (PBs).

  <u>HEVC supports variable PB sizes from 64x64 down to 4x4 samples.</u>

- TUs and transform blocks:

  The prediction residual is coded using block transforms. <u>A TU tree structure has its root at the CU level.</u>

  The luma CB residual may be identical to the luma transform block (TB) or may be further split into smaller luma TBs. The sample applies to the chroma TBs.

  DCT: the square TB sizes 4x4, 8x8, 16x16, and 32x32

  DST: 4x4 luma intra-picutre prediction residuals.

- Motion vector signaling:

  <u>Advanced motion vector prediction (AMVP) is used, including derivation of several most probable candidates based on data from adjacent PBs and the reference picture.</u>

  <u>A merge mode for MV coding can also be used, allowing the inheritance of MVs from temporally or spatially neighboring PBs.</u> 

  Moreover, compared to h.264, improved skipped and direct motion inference are also specified.

- Motion compensation:

  Quarter-sample precision is used for the MV's, and 7-tap or 8-tap filters are used for interpolation of fractional-sample positions. **(h.264: 6-tap filter of half-sample positions followed by linear interpolation for quarter-sample positions)**

  For each PB, either one or two motion vectors can be transmitted. **(same in h.264)**

  A scaling and offset operation may be applied to the prediction signals in a manner known as weighted prediction. **(same in h.264)**

- Intra-picture prediction:

  The decoded boundary samples of adjacent blocks are used as reference data for spatial prediction in regions  where inter-picture prediction is not performed.

  <u>Intra-picture prediction supports 33 directional modes **(8 directional modes in h.264)** , plus planar (surface fitting) and DC (flat) prediction modes.</u>

  The selected intra-picture prediction modes are encoded by deriving most probable modes based on those of previously decoded neighboring PBs.

- Quantization control:

  Uniform reconstruction quantization (URQ) is used with quantization scaling matrices supported for the various transform block sizes. **(same in h.264)**

- Entropy coding:

  Context adaptive binary arithmetic coding (CABAC) is used for entropy coding. **(similar to the CABAC scheme in h.264 but has undergone several improvements to improve its throughput speed (especially for parallel-processing architectures) and its compression performance, and to reduce its context memory requirement)**

- In-loop deblocking filtering:

  A deblocking filter **(similar to the one used in h.264)** is operated <u>within the inter-picture prediction loop</u>. However, the design is simplified in regard to its decision-making and filtering processes, and is made more friendly to parallel processing.

- Sample adaptive offset (SAO):

  A non-linear amplitude mapping is introduced <u>within the inter-picture prediction loop after the deblocking filter.</u>

  Its goal is <u>to better reconstruct the original signal amplitudes</u> by using a look-up table that is described by a few additional parameters that can be determined by histogram analysis at the encoder side.



### High-Level Syntax Architecture

GOALS:

- improve flexibility for operation over a variety of applications and network environments
- improve robustness to data losses.

The high-level syntax architecture used in h.264 has generally been retained, including the following features:

- Parameter set structure:

  Parameter sets contain information that can be shared for the decoding of several regions of the decoded video.

  **SPS, PPS (h.264) -> SPS, PPS, VPS (h.265)**

- NAL unit syntax structure:

  Each syntax structure is placed into a logical data packet called a network abstraction layer (NAL) unit.

  Using the content of a two-byte NAL unit header, it is possible to readily identify the purpose of the associated payload data.

- Slices:

  A slice is a data structure that can be decoded independently from other slices of the same picture. 

  A slice can either be an entire picture or a region of a picture. 

  One of the main purpose of slices is resynchronization in the event of data losses. 

- Supplemental enhancement information (SEI) and video usability information (VUI) metadata:

  This data provides information about the timing of the video pictures, the proper interpretation of the color space used in video signal, 3-D stereoscopic frame packing information, other display hint information, and so on.



### Parallel Decoding Syntax and Modified Slice Structuring

4 new features are introduced in HEVC to enhance the parallel processing capability or modify the structuring of slice data for packetization purposes.

- Tiles: 

  A typical tile configuration of a picture consists of segmenting the picture into <u>rectangular</u> regions with approximately <u>equal numbers of CTUs</u> in each tile.

  <u>Tiles provide parallelism at a more coarse level of granularity and no sophisticated synchronization of threads is necessary for their use.</u>

- Wavefront parallel processing (WPP):

  When WPP is enabled, a slice is divided into rows of CTUs. 

  The first row is processed in an ordinary way, the second row can begin to be processed after only two CTUs have been processed in the first row, the third row can begin to be processed after only two CTUs have been processed in the second row, and so on. 

  WPP provides a form of processing parallelism at a rather fine level of granularity.

- Dependent slice segments:

  

