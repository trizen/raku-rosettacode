[1]: http://rosettacode.org/wiki/4-rings_or_4-squares_puzzle

# [4-rings or 4-squares puzzle][1]

```perl
sub four-squares ( Int :$lo!, Int :$hi! where * > $lo, :$unique=1, :$show=1 ) {
 
    my @solutions;
 
    for $unique.&combos -> @c {
        @solutions.push: @c if [==]
          @c[0] + @c[1],
          @c[1] + @c[2] + @c[3],
          @c[3] + @c[4] + @c[5],
          @c[5] + @c[6];
    }
 
    my $f = "%{$hi.chars}s";
 
    say join "\n", (('a'..'g').fmt: $f), @solutions».fmt($f), '' if $show;
 
    say +@solutions, ($unique ?? ' ' !! ' non-'),
      "unique solutions found using {join(', ', $lo..$hi)}.\n\n";
 
    multi combos ( $ where so *.Bool ) { ($lo..$hi).combinations(7).map: |*.permutations }
 
    multi combos ( $ where not *.Bool ) { [X] ($lo..$hi) xx 7 }
}
 
# TASK
four-squares( :lo(1), :hi(7) );
four-squares( :lo(3), :hi(9) );
four-squares( :lo(0), :hi(9) :unique(0), :show(0) );
```

#### Output:
```
a b c d e f g
3 7 2 1 5 4 6
4 5 3 1 6 2 7
4 7 1 3 2 6 5
5 6 2 3 1 7 4
6 4 1 5 2 3 7
6 4 5 1 2 7 3
7 2 6 1 3 5 4
7 3 2 5 1 4 6

8 unique solutions found using 1, 2, 3, 4, 5, 6, 7.


a b c d e f g
7 8 3 4 5 6 9
8 7 3 5 4 6 9
9 6 4 5 3 7 8
9 6 5 4 3 8 7

4 unique solutions found using 3, 4, 5, 6, 7, 8, 9.


2860 non-unique solutions found using 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.
```