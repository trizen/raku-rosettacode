[1]: http://rosettacode.org/wiki/Pascal's_triangle

# [Pascal's triangle][1]

The following routine returns a lazy list of lines using the sequence operator (`...`). With a lazy result you need not tell the routine how many you want; you can just use a slice subscript to get the first N lines:

```perl
sub pascal { [1], -> $prev { [0, |$prev Z+ |$prev, 0] } ... * }
 
.say for pascal[^10];
```


One problem with the routine above is that it might recalculate the sequence each time you call it. Slightly more idiomatic would be to define the sequence as a lazy constant. Here we use the `@` sigil to indicate that the sequence should cache its values for reuse:

```perl
constant @pascal = [1], -> $prev { [0, |$prev Z+ |$prev, 0] } ... *;
 
.say for @pascal[^10];
```


Since we use ordinary subscripting, non-positive inputs throw an index-out-of-bounds error.

```perl
multi sub pascal (1) { $[1] }
multi sub pascal (Int $n where 2..*) {
    my @rows = pascal $n - 1;
    |@rows, [0, |@rows[*-1] Z+ |@rows[*-1], 0 ];
}
 
.say for pascal 10;
```


Non-positive inputs throw a multiple-dispatch error.

```perl
sub pascal ($n where $n >= 1) {
   say my @last = 1;
   for 1 .. $n - 1 -> $row {
       @last = 1, |map({ @last[$_] + @last[$_ + 1] }, 0 .. $row - 2), 1;
       say @last;
   }
}
 
pascal 10;
```


Non-positive inputs throw a type check error.


#### Output:
```
[1]
[1 1]
[1 2 1]
[1 3 3 1]
[1 4 6 4 1]
[1 5 10 10 5 1]
[1 6 15 20 15 6 1]
[1 7 21 35 35 21 7 1]
[1 8 28 56 70 56 28 8 1]
[1 9 36 84 126 126 84 36 9 1]
```