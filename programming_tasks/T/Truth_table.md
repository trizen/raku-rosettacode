[1]: http://rosettacode.org/wiki/Truth_table

# [Truth table][1]

```perl
use MONKEY-SEE-NO-EVAL;
 
sub MAIN ($x) {
    my @n = $x.comb(/<ident>/);
    my &fun = EVAL "-> {('\\' «~« @n).join(',')} \{ [{ (@n,"so $x").join(',') }] \}";
 
    say (|@n,$x).join("\t");
    .join("\t").say for map &fun, flat map { .fmt("\%0{+@n}b").comb».Int».so }, 0 ..^ 2**@n;
    say '';
}
```

#### Output:
```
$ truthtable 'A ^ B'
A       B       A ^ B
False   False   False
False   True    True
True    False   True
True    True    False

$ truthtable 'foo & bar | baz'
foo     bar     baz     foo & bar | baz
False   False   False   False
False   False   True    True
False   True    False   False
False   True    True    True
True    False   False   False
True    False   True    True
True    True    False   True
True    True    True    True

$ truthtable 'Jim & (Spock ^ Bones) | Scotty'
Jim     Spock   Bones   Scotty  Jim & (Spock ^ Bones) | Scotty
False   False   False   False   False
False   False   False   True    True
False   False   True    False   False
False   False   True    True    True
False   True    False   False   False
False   True    False   True    True
False   True    True    False   False
False   True    True    True    True
True    False   False   False   False
True    False   False   True    True
True    False   True    False   True
True    False   True    True    True
True    True    False   False   True
True    True    False   True    True
True    True    True    False   False
True    True    True    True    True
```
