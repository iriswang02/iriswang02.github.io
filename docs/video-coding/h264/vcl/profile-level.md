# Profile & Level

Profiles and levels specifies conformance points to facilitate interoperability for various applications.

A profile defines a syntax that can be used in generating a conforming bitstream, whereas a level places constraints on the values of key parameters.

Profiles in H.264:

- Baseline (set 0 and 2)
- Main (set 0, 3, 4)
- Extended (set 0, 1, 2, 3)

Flags in the SPS are used to indicate which profiles can decode each video sequence.

Features of the H.264 design can be segmented into the following five elemental sets:

- Set 0 : I and P slices, CAVLC, and other basics
- Set 1: FMO, ASO, and redundant slices
- Set 2: SP/SI slices and slice data partitioning
- Set 3: B slices, weighted prediction, field coding, and macroblock adaptive frame/field coding
- Set 4: CABAC

Fifteen levels are defined, specifying upper limits for picture size, decoder-processing rates, CPB size, DPB size, bitrate, etc.