[1]: https://rosettacode.org/wiki/Super-d_numbers

# [Super-d numbers][1]

### Single-threaded



2 - 6 takes a few seconds, 7 &amp; 8 take a few minutes; I got tired of waiting for 9.

```perl
sub super ($d) {
    my $run = $d x $d;
    "First 10 super-{$d} numbers:\n{ (^∞ .grep: ($d * * ** $d).Str.contains($run) )[^10]}\n"
}
 
put .&super for 2 .. 8
```

#### Output:
```
First 10 super-2 numbers:
19 31 69 81 105 106 107 119 127 131

First 10 super-3 numbers:
261 462 471 481 558 753 1036 1046 1471 1645

First 10 super-4 numbers:
1168 4972 7423 7752 8431 10267 11317 11487 11549 11680

First 10 super-5 numbers:
4602 5517 7539 12955 14555 20137 20379 26629 32767 35689

First 10 super-6 numbers:
27257 272570 302693 323576 364509 502785 513675 537771 676657 678146

First 10 super-7 numbers:
140997 490996 1184321 1259609 1409970 1783166 1886654 1977538 2457756 2714763

First 10 super-8 numbers:
185423 641519 1551728 1854230 6415190 12043464 12147605 15517280 16561735 18542300
```


### Concurrent



Since waiting can be tiresome, make things faster with the `race` concurrency function. The only problem is that we're not sure what the stopping point is ahead of time, so when it seems safe to quit (calculate a few more result than strictly needed, since the parallel workers don't coordinate), break out of the concurrent block by raising an exception. The usual concurrency caveats apply: `@super` must be sized ahead of time, and only modified with an atomic operation (⚛++).



Concurrency makes little difference for d &lt; 6, but the benefits accrue rapidly after that (greater than 10-fold speed-up for d = 8, with an 8-core CPU). However, in the end, you'll still have to wait a quite bit or the super-9 values.

```perl
sub super-d ($d,$max) {
    my $max-plus = $max + floor 2*$max/$d;
    my @super[2*$max-plus];
    {
        my $digits = $d x $d;
        my $chunk  = 250 * $d;
        my atomicint $found = 0;
        (0, 1*$chunk, 2*$chunk ... *).race(:1batch).map: -> $i {
             @super[$found⚛++] = $_ if ($_ ** $d  *  $d).contains($digits) for 1+$i .. 1+$i+$chunk;
             X::AdHoc.new.throw if $found ≥ $max-plus;
        }
        CATCH { when X::AdHoc { return @super.grep(so *).sort.head($max) } }
    }
}
 
say "$_: " ~ join ' ', super-d($_,10) for 2..9;
```

#### Output:
```
2: 19 31 69 81 105 106 107 119 127 131
3: 261 462 471 481 558 753 1036 1046 1471 1645
4: 1168 4972 7423 7752 8431 10267 11317 11487 11549 11680
5: 4602 5517 7539 12955 14555 20137 20379 26629 32767 35689
6: 27257 272570 302693 323576 364509 502785 513675 537771 676657 678146
7: 140997 490996 1184321 1259609 1409970 1783166 1886654 1977538 2457756 2714763
8: 185423 641519 1551728 1854230 6415190 12043464 12147605 15517280 16561735 18542300
9: 17546133 32613656 93568867 107225764 109255734 113315082 121251742 175461330 180917907 182557181
```
