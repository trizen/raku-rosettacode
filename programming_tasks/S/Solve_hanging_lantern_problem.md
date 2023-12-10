[1]: https://rosettacode.org/wiki/Solve_hanging_lantern_problem

# [Solve hanging lantern problem][1]

Note: All of these solutions accept the list of column sizes as command-line arguments and infer the number of columns from the number of sizes provided, rather than requiring that a count be supplied as an extra distinct parameter.



### Directly computing the count



If all we need is the count, then we can compute that directly:

```perl
unit sub MAIN(*@columns);

sub postfix:<!>($n) { [*] 1..$n }

say [+](@columns)! / [*](@columns»!);
```

#### Output:
```
$ raku lanterns.raku 1 2 3
60
```


### Sequence as column numbers



If we want to list all of the sequences, we have to do some more work. This version outputs the sequences as lists of column numbers (assigned from 1 to N left to right); at each step the bottommost lantern from the numbered column is removed.

```perl
unit sub MAIN(*@columns, :v(:$verbose)=False);

my @sequences = @columns
              . pairs
              . map({ (.key+1) xx .value })
              . flat
              . permutations
              . map( *.join(',') )
              . unique;

if ($verbose) {
  say "There are {+@sequences} possible takedown sequences:";
  say "[$_]" for @sequences;
} else {
  say +@sequences;
}
```

#### Output:
```
$ raku lanterns.raku --verbose 1 2 3
There are 60 possible takedown sequences:
[1,2,2,3,3,3]
[1,2,3,2,3,3]
[1,2,3,3,2,3]
[1,2,3,3,3,2]
[1,3,2,2,3,3]
[1,3,2,3,2,3]
...
[3,3,2,2,3,1]
[3,3,2,3,1,2]
[3,3,2,3,2,1]
[3,3,3,1,2,2]
[3,3,3,2,1,2]
[3,3,3,2,2,1]
```


### Sequence as lantern numbers



If we want individually-numbered lanterns in the sequence instead of column numbers, as in the example given in the task description, that requires yet more work:

```perl
unit sub MAIN(*@columns, :v(:$verbose)=False);

my @sequences = @columns
              . pairs
              . map({ (.key+1) xx .value })
              . flat
              . permutations
              . map( *.join(',') )
              . unique;

if ($verbose) {
  my @offsets = |0,|(1..@columns).map: { [+] @columns[0..$_-1] };
  my @matrix;
  for ^@columns.max -> $i {
    for ^@columns -> $j {
      my $value = $i < @columns[$j] ?? ($i+@offsets[$j]+1) !! Nil;
      @matrix[$j][$i] = $value if $value;;
      print "\t" ~ ($value // " ");
    }
    say '';
  }
  say "There are {+@sequences} possible takedown sequences:";
  for @sequences».split(',') -> @seq {
    my @work = @matrix».clone;
    my $seq = '[';
    for @seq -> $col {
      $seq ~= @work[$col-1].pop ~ ',';
    }
    $seq ~~ s/','$/]/;
    say $seq;
  }
} else {
  say +@sequences;
}
```

#### Output:
```
$ raku lanterns.raku -v 1 2 3                                                   
        1       2       4
                3       5
                        6
There are 60 possible takedown sequences:
[1,3,2,6,5,4]
[1,3,6,2,5,4]
[1,3,6,5,2,4]
...
[6,5,4,1,3,2]
[6,5,4,3,1,2]
[6,5,4,3,2,1]
```
