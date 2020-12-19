# Network Abstraction Layer



The NAL is designed to enable simple and effective customization of the use of the VCL for a broad variety of systems. 

##### NAL Units:

The coded video data is organized into NAL units.

The first byte of each NAL unit is a header byte that contains an indication of the type of data in the NAL unit, and the remaining bytes contain payload data of the type indicated by the header.

There are two classes of NAL units:

- VCL NAL units: it contains the data represents the values of the samples in the video pictures.
- non-VCL NAL units: it contains all other related information such as parameter sets and supplemental enhancement information (SEI).

##### Parameter Sets:

A parameter set contains important header information that can apply to a large number of VCL NAL units.

- Sequence Parameter Sets
- Picture Parameter Sets



##### Access Units:

The set of VCL and non-VCL NAL units that is associated with a single decoded picture is referred to as an access unit. 