[1]: http://rosettacode.org/wiki/Comma_quibbling

# [Comma quibbling][1]

```perl
sub comma-quibbling(@A) {
    <{ }>.join: @A < 2 ?? @A !! "@A[0..*-2].join(', ') and @A[*-1]";
}
 
say comma-quibbling($_) for
    [], [<ABC>], [<ABC DEF>], [<ABC DEF G H>];
```

#### Output:
```
{}
{ABC}
{ABC and DEF}
{ABC, DEF, G and H}
```