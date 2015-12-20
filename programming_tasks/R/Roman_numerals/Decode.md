[1]: http://rosettacode.org/wiki/Roman_numerals/Decode

# [Roman numerals/Decode][1]

A non-validating version:

```perl
sub rom-to-num($r) {
    [+] gather $r.uc ~~ /
        ^
        [
        | M  { take 1000 }
        | CM { take 900 }
        | D  { take 500 }
        | CD { take 400 }
        | C  { take 100 }
        | XC { take 90 }
        | L  { take 50 }
        | XL { take 40 }
        | X  { take 10 }
        | IX { take 9 }
        | V  { take 5 }
        | IV { take 4 }
        | I  { take 1 }
        ]+
        $
    /;
}
 
say "$_ => &rom-to-num($_)" for <MCMXC MDCLXVI MMVIII>;
```

#### Output:
```
MCMXC => 1990
MDCLXVI => 1666
MMVIII => 2008
```


A validating version. Also handles older forms such as 'IIXX' and "IIII".

```perl
sub rom-to-num($r) {
    [+] gather $r.uc ~~ /
        ^
        ( (C*)M { take 1000 - 100 * $0.chars } )*
        ( (C*)D { take  500 - 100 * $0.chars } )?
        ( (X*)C { take  100 -  10 * $0.chars } )*
        ( (X*)L { take   50 -  10 * $0.chars } )?
        ( (I*)X { take   10 -       $0.chars } )*
        ( (I*)V { take    5 -       $0.chars } )?
        (     I { take    1                  } )*
        [ $ || { return NaN } ]
    /;
}
 
say "$_ => ", rom-to-num($_) for <MCMXC mdclxvi MMViii IIXX ILL>;
```

#### Output:
```
MCMXC => 1990
mdclxvi => 1666
MMViii => 2008
IIXX => 18
ILL => NaN
```