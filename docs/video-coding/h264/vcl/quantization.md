# Quantization

A quantization parameter (QP) is used for determining the quantization of transform coefficients.

It can take on 52 values. The quantization step size is controlled logarithmically by QP rather than linearly as in previous standards, in a manner designed to reduce decoding complexity and enhance bitrate control capability. Each increase of six in QP causes a doubling of the quantization step size.

The quantized transform coefficients of a block generally are scanned in a *zigzag* fashion and transmitted using entropy coding. The 2x2 DC coefficients of the chroma component are scanned in *raster-scan* order.



All inverse transform operations in H.264 can be implemented using only additions, subtractions, and bit-shifting operations on 16-b integer values, and the scaling can be done using only 16 b as well.

Similarly, only 16-b memory accesses are needed for a good implementation of the forward transform and quantization processes in the encoder.

