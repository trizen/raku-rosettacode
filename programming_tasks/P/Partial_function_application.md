[1]: https://rosettacode.org/wiki/Partial_function_application

# [Partial function application][1]

All Code objects have the .assuming method, which partially applies its arguments. For both type safety reasons and parsing sanity reasons we do not believe in implicit partial application by leaving out arguments. Also, people can understand "assuming" without being steeped in FP culture.

```raku
sub fs ( Code $f, @s ) { @s.map: { .$f } }
 
sub f1 ( $n ) { $n *  2 }
sub f2 ( $n ) { $n ** 2 }
 
my &fsf1 := &fs.assuming(&f1);
my &fsf2 := &fs.assuming(&f2);
 
for [1..3], [2, 4 ... 8] X &fsf1, &fsf2 -> ($s, $f) {
    say $f.($s);
}
```

#### Output:
```
(2 4 6)
(1 4 9)
(4 8 12 16)
(4 16 36 64)
```


The `*+2` is also a form of partial application in Perl&#160;6. In this case we partially apply the `infix:<+>` function with a second argument of 2. That is, the star (known as the "whatever" star) indicates which argument <em>not</em> to apply. In contrast to languages that keep some arguments unbound by leaving holes, the explicit star in Perl&#160;6 allows us to avoid syntactic ambiguity in whether to expect a term or an infix operator; such self-clocking code contributes to better error messages when things go wrong.