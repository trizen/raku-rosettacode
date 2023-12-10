[1]: https://rosettacode.org/wiki/Loops/For_with_a_specified_step

# [Loops/For with a specified step][1]





Depending on how you define your terms, this task is either trivial or impossible in Raku. `for` in Raku doesn't have a step value, it's an iteration operator. It iterates through any iterable object passed to it and sets the topic variable to each iterated value in turn. If you close one eye and squint, I guess you could say it has a step value of 1 (or more accurately, Next) and that isn't really changeable. Whatever iterable you pass to it though can have pretty much any value in pretty much any order you desire, so effectively the "step" value is completely unconstrained.



Examples of iterables (not exhaustive):





Probably the most straightforward way to do this is with a sequence. With at least two values on the left-hand side, the sequence operator (`...`) can infer an arithmetic series. (With at least three values, it can infer a geometric sequence, too.)

```perl
for 2, 4 ... 8 {
    print "$_, ";
}
 
say 'whom do we appreciate?';
```


But there is nothing constraining the sequence to a constant step. Here's one with a *random* step.

```perl
.say for rand, *+rand-.5 ... *.abs>2
```

#### Output:
```
0.1594860240843563
-0.11336537297314198
-0.04195945218519992
-0.024844489074366427
-0.20616093727620433
-0.17589258387167517
-0.40547336592612593
0.04561929494516015
0.4886003890463373
0.7843094215547495
0.6413619589945883
1.0694380727281951
1.472290164849169
1.8310404939418325
2.326272380988639
```


For that matter, the iterated object doesn't need to contain numbers.

```perl
.say for <17/32 π banana 👀 :d(7) 🦋>;
```
