[1]: https://rosettacode.org/wiki/N-queens_problem

# [N-queens problem][1]

Neither pretty nor efficient, a simple backtracking solution

```raku
sub MAIN(\N = 8) {
    sub collision(@field, $row) {
        for ^$row -> $i {
            my $distance = @field[$i] - @field[$row];
            return True if $distance == any(0, $row - $i, $i - $row);
        }
        False;
    }
    sub search(@field, $row) {
        return @field if $row == N;
        for ^N -> $i {
            @field[$row] = $i;
            return search(@field, $row + 1) || next
                unless collision(@field, $row);
        }
        ()
    }
    for 0 .. N / 2 {
        if search [$_], 1 -> @f {
            say @f;
            last;
        }
    }
}
```

#### Output:
```
[0 4 7 5 2 6 1 3]
```