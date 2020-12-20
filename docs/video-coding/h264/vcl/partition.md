# Partition

### Macroblocks:

Each picture is partitioned into fixed size macroblocks that each contain a rectangular picture area of 16x16 samples for the luma component and the corresponding 8x8 samples for each of the two chroma components.

The luma and chroma samples of a macroblock are predicted - either spatially or temporally.

![mb_syntax](./assets/mb_syntax.png)

### Slices:

The macroblocks of the picture are organized into slices, which represent regions of a given picture that can be decoded independently.

Each slice is a sequence of macroblocks that is processed in the order of a raster scan. A picture may contain one or more slices. 

Each slice is self-contained, given the active sequence and PPS, its syntax elements and be parsed from the bitstream and the values of the samples in the area of the picture that the slice represents can basically be decoded without use of data from other slices of the picture.