[1]: https://rosettacode.org/wiki/4-rings_or_4-squares_puzzle

# [4-rings or 4-squares puzzle][1]

```raku
sub four-squares ( @list, :$unique=1, :$show=1 ) {
 
    my @solutions;
 
    for $unique.&combos -> @c {
        @solutions.push: @c if [==]
          @c[0] + @c[1],
          @c[1] + @c[2] + @c[3],
          @c[3] + @c[4] + @c[5],
          @c[5] + @c[6];
    }
 
    say +@solutions, ($unique ?? ' ' !! ' non-'), "unique solutions found using {join(', ', @list)}.\n";
 
    my $f = "%{@list.max.chars}s";
 
    say join "\n", (('a'..'g').fmt: $f), @solutions».fmt($f), "\n" if $show;
 
    multi combos ( $ where so * ) { @list.combinations(7).map: |*.permutations }
 
    multi combos ( $ where not * ) { [X] @list xx 7 }
}
 
# TASK
four-squares( [1..7] );
four-squares( [3..9] );
four-squares( [8, 9, 11, 12, 17, 18, 20, 21] );
four-squares( [0..9], :unique(0), :show(0) );
```

#### Output:
```
8 unique solutions found using 1, 2, 3, 4, 5, 6, 7.

a b c d e f g
3 7 2 1 5 4 6
4 5 3 1 6 2 7
4 7 1 3 2 6 5
5 6 2 3 1 7 4
6 4 1 5 2 3 7
6 4 5 1 2 7 3
7 2 6 1 3 5 4
7 3 2 5 1 4 6


4 unique solutions found using 3, 4, 5, 6, 7, 8, 9.

a b c d e f g
7 8 3 4 5 6 9
8 7 3 5 4 6 9
9 6 4 5 3 7 8
9 6 5 4 3 8 7


8 unique solutions found using 8, 9, 11, 12, 17, 18, 20, 21.

 a  b  c  d  e  f  g
17 21  8  9 11 18 20
20 18 11  9  8 21 17
17 21  9  8 12 18 20
20 18  8 12  9 17 21
20 18 12  8  9 21 17
21 17  9 12  8 18 20
20 18 11  9 12 17 21
21 17 12  9 11 18 20


2860 non-unique solutions found using 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.
```