[1]: https://rosettacode.org/wiki/Halt_and_catch_fire

# [Halt and catch fire][1]

```perl
++8
```


Syntactically: Valid.



Semantically: Change the mathematical concept of 8 to 9, either in your whole computer, or maybe the whole universe.



Fails with this run-time error:


```
Cannot resolve caller prefix:<++>(Int:D); the following candidates
match the type but require mutable arguments:
    (Mu:D $a is rw)
    (Int:D $a is rw --> Int:D)

The following do not match for other reasons:
    (Bool $a is rw)
    (Mu:U $a is rw)
    (Num:D $a is rw)
    (Num:U $a is rw)
    (int $a is rw --> int)
    (num $a is rw --> num)
  in block <unit> at -e line 1
```


Alternately, and perhaps more community condoned, to end the program as soon as possible without trying to change the Laws of the Universe, you could just enter:

```perl
die
```

#### Output:
```
Died
  in block <unit> at <unknown file> line 1
```


Same character count, exits the program as soon as possible (though trappable if desired through the exception system,) and it looks more like an intentional act rather than a typo. Plus, you can add a message that will be added *when* it dies to explain why.



Here is a silly alternative&#160;: A standalone Unicode counterpart for the [yada yada yada](https://docs.raku.org/language/operators#listop_...) operator takes up 3 code units but visually just a single codepoint,


```
cat test.raku ; wc test.raku
…
1 1 4 test.raku

raku -c test.raku ; echo $?
Syntax OK
0

raku test.raku ; echo $?
Stub code executed
  in block <unit> at test.raku line 1

1
```


However when I tried to combine all to test against the Test module, the last one somehow lived through an EVAL,

```perl
use Test;

dies-ok { ++8 };
dies-ok { die };
dies-ok {  …  };

eval-dies-ok '++8';
eval-dies-ok 'die';
eval-dies-ok  '…' ;
```

#### Output:
```
ok 1 -
ok 2 -
ok 3 -
ok 4 -
ok 5 -
not ok 6 -
# Failed test at all.raku line 11
```


so it is indeed at one's discretion whether this one is qualified as a crasher.
