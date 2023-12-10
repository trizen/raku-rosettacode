[1]: https://rosettacode.org/wiki/Non-decimal_radices/Output

# [Non-decimal radices/Output][1]





Calling the `.base` method on a number returns a string. It can handle all bases between 2 and 36:

```perl
say 30.base(2);   # "11110"
say 30.base(8);   # "36"
say 30.base(10);  # "30"
say 30.base(16);  # "1E"
say 30.base(30);  # "10"
```


Alternatively, `printf` can be used for some common number bases:

```perl
for 0..33 -> $n {
  printf " %6b %3o %2d %2X\n", $n xx 4;
}
```
