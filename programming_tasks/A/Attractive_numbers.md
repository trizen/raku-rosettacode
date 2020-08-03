[1]: https://rosettacode.org/wiki/Attractive_numbers

# [Attractive numbers][1]

This algorithm is concise but not really well suited to finding large quantities of consecutive attractive numbers. It works, but isn't especially speedy. More than a hundred thousand or so gets tedious. There are other, much faster (though more verbose) algorithms that *could* be used. This algorithm **is** well suited to finding **arbitrary** attractive numbers though.

```raku
use Prime::Factor;
 
sub display ($n,$m) { ($n..$m).hyper.grep: *.&prime-factors.elems.is-prime }
 
sub count ($n,$m) { +($n..$m).race(:16batch).grep: *.&prime-factors.elems.is-prime }
 
sub comma { $^i.flip.comb(3).join(',').flip }
 
# The Task
put "Attractive numbers from 1 to 120:\n" ~
display(1, 120)».fmt("%3d").rotor(20, :partial).join: "\n";
 
# Robusto!
for 1, 1000,  1, 10000,  2**73 + 1, 2**73 + 100 -> $a, $b {
    put "\nCount of attractive numbers from {comma $a} to {comma $b}:\n" ~
    count $a, $b
}
```

#### Output:
```
Attractive numbers from 1 to 120:
  4   6   8   9  10  12  14  15  18  20  21  22  25  26  27  28  30  32  33  34
 35  38  39  42  44  45  46  48  49  50  51  52  55  57  58  62  63  65  66  68
 69  70  72  74  75  76  77  78  80  82  85  86  87  91  92  93  94  95  98  99
102 105 106 108 110 111 112 114 115 116 117 118 119 120

Count of attractive numbers from 1 to 1,000:
636

Count of attractive numbers from 1 to 10,000:
6396

Count of attractive numbers from 9,444,732,965,739,290,427,393 to 9,444,732,965,739,290,427,492:
58
```