[1]: https://rosettacode.org/wiki/ABC_Problem

# [ABC Problem][1]

Blocks are stored as precompiled regexes. We do an initial pass on the blockset to include in the list only those regexes that match somewhere in the current word. Conveniently, regexes scan the word for us.

```raku
multi can-spell-word(Str $word, @blocks) {
    my @regex = @blocks.map({ my @c = .comb; rx/<@c>/ }).grep: { .ACCEPTS($word.uc) }
    can-spell-word $word.uc.comb.list, @regex;
}
 
multi can-spell-word([$head,*@tail], @regex) {
    for @regex -> $re {
        if $head ~~ $re {
            return True unless @tail;
            return False if @regex == 1;
            return True if can-spell-word @tail, list @regex.grep: * !=== $re;
        }
    }
    False;
}
 
my @b = <BO XK DQ CP NA GT RE TG QD FS JW HU VI AN OB ER FS LY PC ZM>;
 
for <A BaRK BOoK tREaT COmMOn SqUAD CoNfuSE> {
    say "$_     &can-spell-word($_, @b)";
}
```

#### Output:
```
A       True
BaRK    True
BOoK    False
tREaT   True
COmMOn  False
SqUAD   True
CoNfuSE True
```