[1]: https://rosettacode.org/wiki/Phrase_reversals

# [Phrase reversals][1]

```perl
my $s = 'rosetta code phrase reversal';
 
put 'Input               : ', $s;
put 'String reversed     : ', $s.flip;
put 'Each word reversed  : ', $s.words».flip;
put 'Word-order reversed : ', $s.words.reverse;
```

#### Output:
```
Input               : rosetta code phrase reversal
String reversed     : lasrever esarhp edoc attesor
Each word reversed  : attesor edoc esarhp lasrever
Word-order reversed : reversal phrase code rosetta
```