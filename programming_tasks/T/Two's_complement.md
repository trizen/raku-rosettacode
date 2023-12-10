[1]: https://rosettacode.org/wiki/Two%27s_complement

# [Two&#039;s complement][1]

By default Rakus integers are arbitrary sized, theoretically of infinite length. You can't really take the twos complement of an infinitely long number; so, we need to specifically use fixed size integers.



There is a module available from the Raku ecosystem that provides fixed size integer support, named (appropriately enough.) [FixedInt](https://raku.land/zef:thundergnat/FixedInt).



FixedInt supports fixed bit size integers, not only 8 bit, 16 bit, 32 bit or 64 bit, but *ANY* integer size. 22 bit, 35 bit, 191 bit, whatever.



Here we'll demonstrate twos complement on a 57 bit integer.

```perl
use FixedInt;

# Instantiate a new 57(!) bit fixed size integer
my \fixedint = FixedInt.new: :57bits;

fixedint = (2³⁷ / 72 - 5¹⁷); # Set it to a large value

say fixedint;     # Echo the value to the console in decimal format
say fixedint.bin; # Echo the value to the console in binary format

fixedint.=C2;     # Take the twos complement

say fixedint;     # Echo the value to the console in decimal format
say fixedint.bin; # Echo the value to the console in binary format
```

#### Output:
```
144114427045277101
0b111111111111111110100111011001111000010101110110110101101
761030578771
0b000000000000000001011000100110000111101010001001001010011
```
