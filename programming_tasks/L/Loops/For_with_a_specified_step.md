[1]: https://rosettacode.org/wiki/Loops/For_with_a_specified_step

# [Loops/For with a specified step][1]

With at least two values on the left-hand side, the sequence operator (`...`) can infer an arithmetic series. (With at least three values, it can infer a geometric sequence, too.)

```perl
for 2, 4 ... 8 {
    print "$_, ";
}
 
say 'whom do we appreciate?';
```