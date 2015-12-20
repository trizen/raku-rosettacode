[1]: http://rosettacode.org/wiki/Digital_root

# [Digital root][1]

```perl
sub digroot ($r, :$base = 10) {
    my $root = $r.base($base);
    my $persistence = 0;
    while $root.chars > 1 {
        $root = [+]($root.comb.map({:36($_)})).base($base);
        $persistence++;
    }
    $root, $persistence;
}
 
my @testnums =
    627615,
    39390,
    588225,
    393900588225,
    58142718981673030403681039458302204471300738980834668522257090844071443085937;
 
for 10, 8, 16, 36 -> $b {
    for @testnums -> $n {
        printf ":$b\<%s>\ndigital root %s, persistence %s\n\n",
            $n.base($b), digroot $n, :base($b);
    }
}
```

#### Output:
```
:10<627615>
digital root 9, persistence 2

:10<39390>
digital root 6, persistence 2

:10<588225>
digital root 3, persistence 2

:10<393900588225>
digital root 9, persistence 2

:10<58142718981673030403681039458302204471300738980834668522257090844071443085937>
digital root 4, persistence 3

:8<2311637>
digital root 2, persistence 3

:8<114736>
digital root 1, persistence 3

:8<2174701>
digital root 1, persistence 3

:8<5566623376301>
digital root 4, persistence 3

:8<10021347156245115014463623107370014314341751427033746320331121536631531505161175135161>
digital root 3, persistence 3

:16<9939F>
digital root F, persistence 2

:16<99DE>
digital root F, persistence 2

:16<8F9C1>
digital root F, persistence 2

:16<5BB64DFCC1>
digital root F, persistence 2

:16<808B9CDCA526832679323BE018CC70FA62E1BF3341B251AF666B345389F4BA71>
digital root 7, persistence 3

:36<DG9R>
digital root U, persistence 2

:36<UE6>
digital root F, persistence 2

:36<CLVL>
digital root F, persistence 2

:36<50YE8N29>
digital root P, persistence 2

:36<37C71GOYNYJ25M3JTQQVR0FXUK0W9QM71C1LVNCBWNRVNOJYPD>
digital root H, persistence 3
```


Or if you are more inclined to the functional programming persuasion, you can use the `...` sequence operator to calculate the values without side effects:

```perl
sub digroot ($r, :$base = 10) {
    my &sum = { [+](.comb.map({:36($_)})).base($base) }
 
    return .[*-1], .elems-1
        given $r.base($base), &sum ...  { .chars == 1 }
}
```