[1]: https://rosettacode.org/wiki/Extend_your_language

# [Extend your language][1]


Writing the conditional blocks is no problem, since there's no distinction between built-in closures and user-defined.  The `if2` is currently just a function call, which requires a comma after the second condition; eventually we will be able to drop that in user-defined code, but none of our implementations can define parse actions in the statement\_control category quite yet.  (That syntax is special insofar as it's the only place Raku allows two terms in a row.)  So currently it requires the comma until the implementations catch up with the spec on that subject.



This solution is hygienic in both lexical and dynamic variables; the only caveat is that the user's program must avoid the dynamic variable being used by the implementation of conditional, `$*IF2`.  This does not seem like a great hardship.  (The conditionals will also nest correctly since that's how dynamic variables behave.)

```perl
my &if2  = -> \a, \b, &x { my @*IF2 = ?a,?b; x }

my &if-both    = -> &x { x if @*IF2 eq (True,True)  }
my &if-first   = -> &x { x if @*IF2 eq (True,False) }
my &if-second  = -> &x { x if @*IF2 eq (False,True) }
my &if-neither = -> &x { x if @*IF2 eq (False,False)}

sub test ($a,$b) {
    $_ = "G";          # Demo correct scoping of topic.
    my $got = "o";     # Demo correct scoping of lexicals.
    my $*got = "t";    # Demo correct scoping of dynamics.

    if2 $a, $b, {
        if-both { say "$_$got$*got both" }
        if-first { say "$_$got$*got first" }
        if-second { say "$_$got$*got second" }
        if-neither { say "$_$got$*got neither" }
    }
}

say test |$_ for 1,0 X 1,0;
```

#### Output:
```
Got both
Got first
Got second
Got neither
```
