[1]: http://rosettacode.org/wiki/Non-decimal_radices/Output

# [Non-decimal radices/Output][1]

```perl6
for 0..33 -> $n {
  printf " %6b %3o %2d %2X\n", $n xx 4;
}
```