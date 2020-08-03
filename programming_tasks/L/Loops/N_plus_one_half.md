[1]: https://rosettacode.org/wiki/Loops/N_plus_one_half

# [Loops/N plus one half][1]

```raku
for 1 .. 10 {
    .print;
    last when 10;
    print ', ';
}
 
print "\n";
```