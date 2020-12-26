# Transform

The transformation is applied to **4x4 blocks** and <u>a separable integer transform with similar properties to a 4x4 DCT</u> is used.

Since the inverse transform is defined by very simple exact integer operations, inverse-transform mismatches are avoided and decoding complexity is minimized.

Several reasons for using a smaller transform size (4x4):

- Since the prediction process for Inter and Intra has been improved, the residual signal has less spatial correlation. This generally means that the transform has less to offer concerning decorrelation, so a 4x4 transform is essentially as efficient.
- the smaller 4x4 transform has visual benefits resulting in less noise around edges ("ringing" artifacts)
- less computation and a smaller processing word length.

For the luma component in the Intra_16x16 mode and for the chroma components in all Intra macroblocks, the DC coefficients of the 4x4 transform blocks undergo a second transform to <u>improve compression performance for very smooth regions.</u>

This additional transform is 4x4 for the processing of the luma component in Intra_16x16 mode and is 2x2 for the processing of each chroma component in all Intra modes.

