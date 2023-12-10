[1]: https://rosettacode.org/wiki/Fusc_sequence

# [Fusc sequence][1]





### Recurrence

```perl
my @Fusc = 0, 1, 1, { |(@Fusc[$_ - 1] + @Fusc[$_], @Fusc[$_]) given ++$+1 } ... *;

sub comma { $^i.flip.comb(3).join(',').flip }

put "First 61 terms of the Fusc sequence:\n{@Fusc[^61]}" ~
    "\n\nIndex and value for first term longer than any previous:";

for flat 'Index', 'Value', 0, 0, (1..4).map({
    my $l = 10**$_;
    @Fusc.first(* > $l, :kv).map: &comma
  }) -> $i, $v {
      printf "%15s : %s\n", $i, $v
}
```

#### Output:
```
First 61 terms of the Fusc sequence:
0 1 1 2 1 3 2 3 1 4 3 5 2 5 3 4 1 5 4 7 3 8 5 7 2 7 5 8 3 7 4 5 1 6 5 9 4 11 7 10 3 11 8 13 5 12 7 9 2 9 7 12 5 13 8 11 3 10 7 11 4

Index and value for first term longer than any previous:
          Index : Value
              0 : 0
             37 : 11
          1,173 : 108
         35,499 : 1,076
        699,051 : 10,946
```


### Recursive



Alternative version using Raku's multiple-dispatch feature. This version is significantly slower than the one above, but it's definitely prettier.

```perl
multi fusc( 0 ) { 0 }
multi fusc( 1 ) { 1 }
multi fusc( $n where $n %% 2 ) { fusc $n div 2 }
multi fusc( $n ) { [+] map *.&fusc, ( $n - 1 ) div 2, ( $n + 1 ) div 2 }
put map *.&fusc, 0..60;
```

#### Output:
```
0 1 1 2 1 3 2 3 1 4 3 5 2 5 3 4 1 5 4 7 3 8 5 7 2 7 5 8 3 7 4 5 1 6 5 9 4 11 7 10 3 11 8 13 5 12 7 9 2 9 7 12 5 13 8 11 3 10 7 11 4
```
