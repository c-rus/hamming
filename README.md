# `hamming`

## Overview

A generic VHDL implementation for encoding and decoding of the error-correction hamming code.

The implementation uses the "extended" hamming-code, where the 0th bit is an additional parity check against the entire data block for double-error detection (DED). 

A single erroneous bit can be corrected for each hamming-code block (SEC). Any block received with an even number of errors `e` with `e` > 0 will not be valid. A block received with an odd number of errors `e` with `e` > 1 may self-correct to the incorrect original message.

`PARITY_BITS` | Block size | Data size  | Rate  
---     | --- | --- | --- 
2       | 4   | 1   | 1/4 = 0.250
3       | 8   | 4   | 4/8 = 0.500
4       | 16  | 11  | 11/16 ≈ 0.688
5       | 32  | 26  | 26/32 ≈ 0.813
6       | 64  | 57  | 57/64 ≈ 0.891
7       | 128 | 120 | 120/128 ≈ 0.938
8       | 256 | 247 | 247/256 ≈ 0.965

> __Note:__ `PARITY_BITS` does not account for the 0th extended parity bit. It is implicitly added to the block size.

## Testing

To test the hamming-code software model:
```
python -m unittest discover ./sim/mdl hamming.py
```

To test the `parity` entity:
```
orbit plan --clean --top parity --plugin ghdl
orbit b -- -g SIZE=9 
orbit b -- -g SIZE=4 -g EVEN_PARITY=false
```

To test the `decoder` entity:
```
orbit plan --clean --plugin ghdl --top decoder
orbit b
orbit b -- -g SIZE=4
```

To test the `hamm_enc` entity:
```
orbit plan --clean --plugin ghdl --top hamm_enc
orbit b
orbit b -- -g PARITY_BITS=4
```

To test the `hamm_dec` entity:
```
orbit plan --clean --plugin ghdl --top hamm_dec
orbit b
orbit b -- -g PARITY_BITS=6
orbit b -- -g PARITY_BITS=2
```

## Reference

- "How to send a self-correcting message (Hamming codes)" - 3Blue1Brown  
https://www.youtube.com/watch?v=X8jsijhllIA

- "Hamming code" - Wikipedia  
https://en.wikipedia.org/wiki/Hamming_code#[7,4]_Hamming_code

- "Parity bit" - Wikipedia  
https://en.wikipedia.org/wiki/Parity_bit