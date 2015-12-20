[1]: http://rosettacode.org/wiki/Levenshtein_distance/Alignment

# [Levenshtein distance/Alignment][1]

```perl
sub align ( Str $σ, Str $t ) {
    my @[s](http://perldoc.perl.org/functions/s.html) = *, $σ.comb;
    my @t = *, $t.comb;
 
    my @A;
    @A[$_][ 0]<d [s](http://perldoc.perl.org/functions/s.html) t> = $_, @[s](http://perldoc.perl.org/functions/s.html)[1..$_].[join](http://perldoc.perl.org/functions/join.html), '-' x $_ for ^@[s](http://perldoc.perl.org/functions/s.html);
    @A[ 0][$_]<d [s](http://perldoc.perl.org/functions/s.html) t> = $_, '-' x $_, @t[1..$_].[join](http://perldoc.perl.org/functions/join.html) for ^@t;
 
    for 1 ..^ @[s](http://perldoc.perl.org/functions/s.html) X 1..^ @t -> \i, \j {
	if @[s](http://perldoc.perl.org/functions/s.html)[i] ne @t[j] {
	    @A[i][j]<d> = 1 + my $min =
	    min @A[i-1][j]<d>, @A[i][j-1]<d>, @A[i-1][j-1]<d>;
	    @A[i][j]<[s](http://perldoc.perl.org/functions/s.html) t> =
	    @A[i-1][j]<d> == $min ??  (@A[i-1][j]<[s](http://perldoc.perl.org/functions/s.html) t> Z~ @[s](http://perldoc.perl.org/functions/s.html)[i], '-') !!
	    @A[i][j-1]<d> == $min ??  (@A[i][j-1]<[s](http://perldoc.perl.org/functions/s.html) t> Z~ '-', @t[j]) !!
	    (@A[i-1][j-1]<[s](http://perldoc.perl.org/functions/s.html) t> Z~ @[s](http://perldoc.perl.org/functions/s.html)[i], @t[j]);
	} else {
	    @A[i][j]<d [s](http://perldoc.perl.org/functions/s.html) t> = @A[i-1][j-1]<d [s](http://perldoc.perl.org/functions/s.html) t> Z~ '', @[s](http://perldoc.perl.org/functions/s.html)[i], @t[j];
	}
    }
 
    [return](http://perldoc.perl.org/functions/return.html) @A[*-1][*-1]<[s](http://perldoc.perl.org/functions/s.html) t>;
}
 
.say for align |<rosettacode raisethysword>;
```

#### Output:
```
ro-settac-o-de
raisethysword-
```