[1]: http://rosettacode.org/wiki/Longest_common_prefix

# [Longest common prefix][1]

This should work on infinite strings (if and when we get them), since <tt>.ords</tt> is lazy. In any case, it does just about the minimal work by evaluating all strings lazily in parallel. A few explanations of the juicy bits: <tt>\@s</tt> is the list of strings, and the hyper operator <tt>»</tt> applies the <tt>.ords</tt> to each of those strings, producing a list of lists. The <tt>|</tt> operator inserts each of those sublists as an argument into an argument list so that we can use a reduction operator across the list of lists, which makes sense if the operator in question knows how to deal with list arguments. In this case we use the <tt>Z</tt> ('zip') metaoperator with <tt>eqv</tt> as a base operator, which runs <tt>eqv</tt> across all the lists in parallel for each position, and will fail if not all the lists have the same ordinal value at that position, or if any of the strings run out of characters. Then we count up the leading matching positions and carve up one of the strings to that length.

```perl
multi lcp()    { '' }
multi lcp($s)  { ~$s }
multi lcp(*@s) { substr @s[0], 0, [+] [\and] [Zeqv] |@s».ords }
 
use Test;
plan 8;
 
is lcp("interspecies","interstellar","interstate"), "inters";
is lcp("throne","throne"), "throne";
is lcp("throne","dungeon"), '';
is lcp("cheese"), "cheese";
is lcp(''), '';
is lcp(), '';
is lcp("prefix","suffix"), '';
is lcp("foo","foobar"), 'foo';
```

#### Output:
```
1..8
ok 1 - 
ok 2 - 
ok 3 - 
ok 4 - 
ok 5 - 
ok 6 - 
ok 7 - 
ok 8 - 
```