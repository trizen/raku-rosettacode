[1]: https://rosettacode.org/wiki/Loops/With_multiple_ranges

# [Loops/With multiple ranges][1]





This task is really conflating two separate things, (at least in Raku). Sequences and loops are two different concepts and may be considered / implemented separately from each other.



Yes, you can generate a sequence with a loop, and a loop can use a sequence for an iteration value, but the two are somewhat orthogonal and don't necessarily overlap.



Sequences are first class objects in Raku. You can (and typically do) generate a sequence using the (appropriately enough) sequence operator and can assign it to a variable and/or pass it as a parameter; the entire sequence, not just it's individual values. It *may* be used in a looping construct, but it is not necessary to do so.



Various looping constructs often do use sequences as their iterator but not exclusively, possibly not even in the majority.





Displaying the j sequence as well since it isn't very large.

```perl
sub comma { ($^i < 0 ?? '-' !! '') ~ $i.abs.flip.comb(3).join(',').flip }
 
my \x     =  5;
my \y     = -5;
my \z     = -2;
my \one   =  1;
my \three =  3;
my \seven =  7;
 
my $j = flat
  ( -three, *+three … 3³         ),
  ( -seven, *+x     …^ * > seven ),
  ( 555   .. 550 - y             ),
  ( 22,     *-three …^ * < -28   ),
  ( 1927  .. 1939                ),
  ( x,      *+z     …^ * < y     ),
  ( 11**x .. 11**x + one         );
 
put 'j sequence: ', $j;
put '       Sum: ', comma [+] $j».abs;
put '   Product: ', comma ([\*] $j.grep: so +*).first: *.abs > 2²⁷;
 
# Or, an alternate method for generating the 'j' sequence, employing user-defined
# operators to preserve the 'X to Y by Z' layout of the example code.
# Note that these operators will only work for monotonic sequences.
 
sub infix:<to> { $^a ... $^b }
sub infix:<by> { $^a[0, $^b.abs ... *] }
 
$j = cache flat
    -three  to          3**3  by  three ,
    -seven  to         seven  by      x ,
       555  to     (550 - y)            ,
        22  to           -28  by -three ,
      1927  to          1939  by    one ,
         x  to             y  by      z ,
     11**x  to (11**x + one)            ;
 
put "\nLiteral minded variant:";
put '       Sum: ', comma [+] $j».abs;
put '   Product: ', comma ([\*] $j.grep: so +*).first: *.abs > 2²⁷;
```

#### Output:
```
j sequence: -3 0 3 6 9 12 15 18 21 24 27 -7 -2 3 555 22 19 16 13 10 7 4 1 -2 -5 -8 -11 -14 -17 -20 -23 -26 1927 1928 1929 1930 1931 1932 1933 1934 1935 1936 1937 1938 1939 5 3 1 -1 -3 -5 161051 161052
       Sum: 348,173
   Product: -793,618,560

Literal minded variant:
       Sum: 348,173
   Product: -793,618,560
```
