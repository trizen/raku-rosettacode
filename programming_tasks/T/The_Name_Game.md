[1]: https://rosettacode.org/wiki/The_Name_Game

# [The Name Game][1]

Meh. The rules leave out some corner cases (see Steve) but what the heck, technically correct is the best kind of correct.

```perl
sub mangle ($name, $initial) {
    my $fl = $name.lc.substr(0,1);
    $fl ~~ /<[aeiou]>/
    ?? $initial~$name.lc
    !! $fl eq $initial
    ?? $name.substr(1)
    !! $initial~$name.substr(1)
}
 
sub name-game (Str $name) {
    qq:to/NAME-GAME/;
    $name, $name, bo-{ mangle $name, 'b' }
    Banana-fana fo-{ mangle $name, 'f' }
    Fee-fi-mo-{ mangle $name, 'm' }
    $name!
    NAME-GAME
}
 
say .&name-game for <Gary Earl Billy Felix Mike Steve>
```

#### Output:
```
Gary, Gary, bo-bary
Banana-fana fo-fary
Fee-fi-mo-mary
Gary!

Earl, Earl, bo-bearl
Banana-fana fo-fearl
Fee-fi-mo-mearl
Earl!

Billy, Billy, bo-illy
Banana-fana fo-filly
Fee-fi-mo-milly
Billy!

Felix, Felix, bo-belix
Banana-fana fo-elix
Fee-fi-mo-melix
Felix!

Mike, Mike, bo-bike
Banana-fana fo-fike
Fee-fi-mo-ike
Mike!

Steve, Steve, bo-bteve
Banana-fana fo-fteve
Fee-fi-mo-mteve
Steve!
```