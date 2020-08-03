[1]: https://rosettacode.org/wiki/Population_count

# [Population count][1]

```perl
sub population-count(Int $n where * >= 0) { [+] $n.base(2).comb }
 
say map &population-count, 3 «**« ^30;
say "Evil: ", (grep { population-count($_) %% 2 }, 0 .. *)[^30];
say "Odious: ", (grep { population-count($_)  % 2 }, 0 .. *)[^30];
```

#### Output:
```
1 2 2 4 3 6 6 5 6 8 9 13 10 11 14 15 11 14 14 17 17 20 19 22 16 18 24 30 25 25
Evil: 0 3 5 6 9 10 12 15 17 18 20 23 24 27 29 30 33 34 36 39 40 43 45 46 48 51 53 54 57 58
Odious: 1 2 4 7 8 11 13 14 16 19 21 22 25 26 28 31 32 35 37 38 41 42 44 47 49 50 52 55 56 59
```


That's the convenient way to write it, but the following avoids string processing and is therefore about twice as fast:

```perl
sub population-count(Int $n is copy where * >= 0) { 
    loop (my $c = 0; $n; $n +>= 1) { 
        $c += $n +& 1; 
    } 
    $c;
}
```