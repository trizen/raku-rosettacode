[1]: http://rosettacode.org/wiki/Truth_table

# [Truth table][1]

```perl
sub MAIN ($x) {
    my @n = $x.comb(/<ident>/);
    my &fun = eval "-> {('\\' «~« @n).join(',')} \{ [{ (@n,"so $x").join(',') }] \}";
 
    say (@n,$x).join("\t");
    .join("\t").say for map &fun, map { .fmt("\%0{+@n}b").comb».so }, 0 ..^ 2**@n;
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