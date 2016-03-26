[1]: http://rosettacode.org/wiki/Amb

# [Amb][1]

Junctions are a construct that behave similarly to the wanted Amb operator. The only difference is, that they don't preserve the state that was True inside any control structure (like an if).



There is currently a trick, how you only get the "true" values from a Junction for any test: return from a subroutine. Because of DeMorgans Law, you'll have to switch and and or, since you want to return on falseness. Just look at 'all' in combination with the sub(){return unless test} as the amb operator.

```perl
 
#| an array of four words, that have more possible values. 
#| Normally we would want `any' to signify we want any of the values, but well negate later and thus we need `all'
my @a =
(all «the that a»),
(all «frog elephant thing»),
(all «walked treaded grows»),
(all «slowly quickly»);
 
sub test (Str $l, Str $r) {
    $l.ends-with($r.substr(0,1))
}
 
(sub ($w1, $w2, $w3, $w4){
  # return if the values are false
  return unless [and] test($w1, $w2), test($w2, $w3),test($w3, $w4);
  # say the results. If there is one more Container layer around them this doesn't work, this is why we need the arguments here.
  say "$w1 $w2 $w3 $w4"
})(|@a); # supply the array as argumetns
 
 
```





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





*Note: the compiler suggests adding `use MONKEY-SEE-NO-EVAL;` to enable regex interpolation, but that's not the only issue. The program outputs nothing.*

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