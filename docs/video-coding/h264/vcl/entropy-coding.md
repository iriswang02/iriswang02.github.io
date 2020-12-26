# Entropy Coding



- Context-adaptive binary variable-length coding (CAVLC): VLC tables are switched depending on the values of previous syntax elements. Since the VLC tables are context conditional, the coding efficiency is better than for schemes using a single VLC table.
- Context-adaptive binary arithmetic coding (CABAC): CABAC is not only uses context-conditional probability estimates, but adjusts its probability estimates to adapt to nonstationary statistical behavior. The arithmetic coding core engine and its associated probability estimation use low-complexity multiplication-free operations involving only shifts and table lookups.

CABAC has higher complexity than CAVLC, but has better coding efficiency. Compared to CAVLC, CABAC typically reduces the bitrate 10%-15% for the same quality.

In both of these modes, many syntax elements are coded using a single infinite-extent codeword set referred to as an *Exp-Golomb* code.

