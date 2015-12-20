[1]: http://rosettacode.org/wiki/Parsing/Shunting-yard_algorithm

# [Parsing/Shunting-yard algorithm][1]

```perl6
my %prec =
    '^' => 4,
    '*' => 3,
    '/' => 3,
    '+' => 2,
    '-' => 2,
    '(' => 1;
 
my %assoc =
    '^' => 'right',
    '*' => 'left',
    '/' => 'left',
    '+' => 'left',
    '-' => 'left';
 
sub shunting-yard ($prog) {
    my @inp = $prog.words;
    my @ops;
    my @res;
 
    sub report($op) { printf "%25s    %-7s %10s %s\n", ~@res, ~@ops, $op, ~@inp }
    sub shift($t)  { report( "shift $t"); @ops.push: $t }
    sub reduce($t) { report("reduce $t"); @res.push: $t }
 
    while @inp {
	given @inp.shift {
	    when /\d/ { reduce $_ };
	    when '(' { shift $_ }
	    when ')' { while @ops and (my $x = @ops.pop and $x ne '(') { reduce $x } }
	    default {
		my $newprec = %prec{$_};
		while @ops {
		    my $oldprec = %prec{@ops[*-1]};
		    last if $newprec > $oldprec;
		    last if $newprec == $oldprec and %assoc{$_} eq 'right';
		    reduce @ops.pop;
		}
		shift $_;
	    }
	}
    }
    reduce @ops.pop while @ops;
    @res;
}
 
say shunting-yard '3 + 4 * 2 / ( 1 - 5 ) ^ 2 ^ 3';
```

#### Output:
```
                                       reduce 3 + 4 * 2 / ( 1 - 5 ) ^ 2 ^ 3
                        3               shift + 4 * 2 / ( 1 - 5 ) ^ 2 ^ 3
                        3    +         reduce 4 * 2 / ( 1 - 5 ) ^ 2 ^ 3
                      3 4    +          shift * 2 / ( 1 - 5 ) ^ 2 ^ 3
                      3 4    + *       reduce 2 / ( 1 - 5 ) ^ 2 ^ 3
                    3 4 2    +         reduce * ( 1 - 5 ) ^ 2 ^ 3
                  3 4 2 *    +          shift / ( 1 - 5 ) ^ 2 ^ 3
                  3 4 2 *    + /        shift ( 1 - 5 ) ^ 2 ^ 3
                  3 4 2 *    + / (     reduce 1 - 5 ) ^ 2 ^ 3
                3 4 2 * 1    + / (      shift - 5 ) ^ 2 ^ 3
                3 4 2 * 1    + / ( -   reduce 5 ) ^ 2 ^ 3
              3 4 2 * 1 5    + / (     reduce - ^ 2 ^ 3
            3 4 2 * 1 5 -    + /        shift ^ 2 ^ 3
            3 4 2 * 1 5 -    + / ^     reduce 2 ^ 3
          3 4 2 * 1 5 - 2    + / ^      shift ^ 3
          3 4 2 * 1 5 - 2    + / ^ ^   reduce 3 
        3 4 2 * 1 5 - 2 3    + / ^     reduce ^ 
      3 4 2 * 1 5 - 2 3 ^    + /       reduce ^ 
    3 4 2 * 1 5 - 2 3 ^ ^    +         reduce / 
  3 4 2 * 1 5 - 2 3 ^ ^ /              reduce + 
3 4 2 * 1 5 - 2 3 ^ ^ / +
```