[1]: https://rosettacode.org/wiki/Longest_common_substring

# [Longest common substring][1]



```perl
sub createSubstrings( Str $word --> Array ) {
   my $length = $word.chars ;
   my @substrings ;
   for (0..$length - 1) -> $start {
      for (1..$length - $start) -> $howmany {
	 @substrings.push( $word.substr( $start , $howmany ) ) ;
      }
   }
   return @substrings ;
}

sub findLongestCommon( Str $first , Str $second --> Str ) {
   my @substringsFirst  = createSubstrings( $first ) ;
   my @substringsSecond = createSubstrings( $second ) ;
   my $firstset  = set( @substringsFirst ) ;
   my $secondset = set( @substringsSecond ) ;
   my $common = $firstset (&) $secondset ;
   return $common.keys.sort({$^b.chars <=> $^a.chars})[0] ; # or: sort(-*.chars)[0]
}

sub MAIN( Str $first , Str $second ) {
   say "The longest common substring of $first and $second is " ~
   "{findLongestCommon( $first , $second ) } !" ;
}
```

#### Output:
```
The longest common substring of thisisatest and testing123testing is test !
```


### Functional

```perl
sub substrings ($s)       { (flat (0..$_ X 1..$_).grep:{$_ ≥ [+] @_}).map: { $s.substr($^a, $^b) } given $s.chars }
sub infix:<LCS>($s1, $s2) { ([∩] ($s1, $s2)».&substrings).keys.sort(*.chars).tail }

my $first  = 'thisisatest';
my $second = 'testing123testing';
say "The longest common substring between '$first' and '$second' is '{$first LCS $second}'.";
```

#### Output:
```
The longest common substring between 'thisisatest' and 'testing123testing' is 'test'.
```
