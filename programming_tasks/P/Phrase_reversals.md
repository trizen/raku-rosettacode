[1]: http://rosettacode.org/wiki/Phrase_reversals

# [Phrase reversals][1]

```perl
my $s = 'rosetta code phrase reversal';
 
say 'Input               : ', $s;
say 'String reversed     : ', $s.flip;
say 'Each word reversed  : ', $s.words».flip;
say 'Word-order reversed : ', $s.words.reverse;
```

#### Output:
```
Input               : rosetta code phrase reversal
String reversed     : lasrever esarhp edoc attesor
Each word reversed  : attesor edoc esarhp lasrever
Word-order reversed : reversal phrase code rosetta
```