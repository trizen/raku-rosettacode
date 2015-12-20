[1]: http://rosettacode.org/wiki/Amb

# [Amb][1]

```perl
sub infix:<lf> ($a,$b) {
    next unless try $a.substr(*-1,1) eq $b.substr(0,1);
    "$a $b";
}
 
multi dethunk(Callable $x) { try take $x() }
multi dethunk(     Any $x) {     take $x   }
 
sub amb (*@c) { gather @c».&dethunk }
 
say first *, do
    amb(<the that a>, { die 'oops'}) Xlf
    amb('frog',{'elephant'},'thing') Xlf
    amb(<walked treaded grows>)      Xlf
    amb { die 'poison dart' },
        {'slowly'},
        {'quickly'},
        { die 'fire' };
```

#### Output:
```
that thing grows slowly
```


This uses lazy lists, created by the `X` metaoperator applied to a user-defined function, `lf`, that asserts the last-first condition,
and short-circuits the match so that it does not need to generate parts of the search tree that cannot match. We use the `first` function to pull one element from the lazy list; a subscript of `[0]` would have worked just as well.



The `amb` operator itself uses a hyper to run the `dethunk` calls in parallel. Results are returned asyncronously via `gather`/`take`. The `dethunk` call traps failures after the failure has bypassed the `take`.



If you consider lazy lists to be cheating on the idea of continuations, here's
some admittedly grungy code that uses the continuation engine of regexes to solve it. At some point we'll wrap this up in nice syntax to let people write in a sublanguage of Perl 6 that looks more like a logic language.

```perl
sub amb($var,*@a) {
    "[{
        @a.pick(*).map: {"||\{ $var = '$_' }"}
     }]";
}
 
sub joins ($word1, $word2) {
    substr($word1,*-1,1) eq substr($word2,0,1)
}
 
'' ~~ m/
    :my ($a,$b,$c,$d);
    <{ amb '$a', <the that a> }>
    <{ amb '$b', <frog elephant thing> }>
    <?{ joins $a, $b }>
    <{ amb '$c', <walked treaded grows> }>
    <?{ joins $b, $c }>
    <{ amb '$d', <slowly quickly> }>
    <?{ joins $c, $d }>
    { say "$a $b $c $d" }
    <!>
/;
```