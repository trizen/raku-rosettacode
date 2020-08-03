[1]: https://rosettacode.org/wiki/Nonoblock

# [Nonoblock][1]

```raku
for (5, [2,1]), (5, []), (10, [8]), (5, [2,3]), (15, [2,3,2,3]) -> ($cells, @blocks) {
    say $cells, ' cells with blocks: ', @blocks ?? join ', ', @blocks !! '∅';
    my $letter = 'A';
    my $row = join '.', map { $letter++ x $_ }, @blocks;
    say "no solution\n" and next if $cells < $row.chars;
    say $row ~= '.' x $cells - $row.chars;
    say $row while $row ~~ s/^^ (\.*) <|w> (.*?) <|w> (\w+) \.<!|w> /$1$0.$2/;
    say '';
}
```

#### Output:
```
5 cells with blocks: 2, 1
AA.B.
AA..B
.AA.B

5 cells with blocks: ∅
.....

10 cells with blocks: 8
AAAAAAAA..
.AAAAAAAA.
..AAAAAAAA

5 cells with blocks: 2, 3
no solution

15 cells with blocks: 2, 3, 2, 3
AA.BBB.CC.DDD..
AA.BBB.CC..DDD.
AA.BBB..CC.DDD.
AA..BBB.CC.DDD.
.AA.BBB.CC.DDD.
AA.BBB.CC...DDD
AA.BBB..CC..DDD
AA..BBB.CC..DDD
.AA.BBB.CC..DDD
AA.BBB...CC.DDD
AA..BBB..CC.DDD
.AA.BBB..CC.DDD
AA...BBB.CC.DDD
.AA..BBB.CC.DDD
..AA.BBB.CC.DDD
```