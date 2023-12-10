[1]: https://rosettacode.org/wiki/Topic_variable

# [Topic variable][1]





As in Perl, in Raku the topic variable is $\_.  In addition to a direct assigment, it can be set with the 'given' or 'with' keywords, or by some iteration operator ('for', 'map' etc).  A method can be called from it with an implicit call:

```perl
$_ = 'Outside';
for <3 5 7 10> {
    print $_;
    .³.map: { say join "\t", '', $_, .², .sqrt, .log(2), OUTER::<$_>, UNIT::<$_>  }
}
```

#### Output:
```
3       27      729     5.196152422706632       4.754887502163469       3       Outside
5       125     15625   11.180339887498949      6.965784284662088       5       Outside
7       343     117649  18.520259177452136      8.422064766172811       7       Outside
10      1000    1000000 31.622776601683793      9.965784284662087       10      Outside
```


Note: that the $\_ inside the 'map' block is different than the $\_ set in the 'for' block. The scope of the `$_` variable is always lexical in Raku, though a function can access topics in other scopes by specifying the scope: `CALLER::<$_>` from within a subroutine, `OUTER::<$_>` for the next enclosing block, `UNIT::<$_>` for the outermost block in a compilation unit, etc. The scope modifiers may be stacked: `CALLER::OUTER::<$_>` would be the topic variable of the next outer block of the current subroutines caller block.



The scope modifiers can only flow outward. There is no way to access the topic of a block enclosed within the current scope or in a separate scope 'branch'.



Also note: **Any** variable in other scopes can be accessed this way. This is just pointing out that the default (topic) variable also has this property.
