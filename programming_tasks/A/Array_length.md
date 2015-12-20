[1]: http://rosettacode.org/wiki/Array_length

# [Array length][1]

To get the number of elements of an array in Perl 6 you put the array in a coercing Numeric context, or call `elems` on it.

```perl6
my @array = |<apple orange banana>, set(<a b c>), 42;
 
+@array;
 
prefix:<+>   @array; # ditto, using the subroutine name of the operator
 
+[<apple orange>];
+(42,); # a comma creates a "List", the parens just disambiguate
 
my $arr = @array;    +$arr; # `$arr` *holds* an alias to @array
my \arr = @array;    +arr;  # `arr` *is* an alias to @array
 
0 + @array;
 
5 == @array; # coerces to the number of elements then compares it against 5
# ( use `eqv` for comparing the elements of two arrays )
 
@array.elems;
elems @array;
 
@array.Numeric; # or any of «Real Int Num Rat»
 
 
# takes a value and coerces "Any" value to a "Real" "Numeric" value
sub takes-a-real ( Real() $i ){ say $i }
 
takes-a-real @array;
```


Watch out for infinite/lazy arrays though.

```perl6
my @twos = 1, 2, 4, 8 ... *; # «1 2 4 8 16 32 64 128» ... Int.max, Inf, Inf, Inf ... *
 
say +@twos; # would be an infinite loop ( actually an error )
 
loop (
  my Int:D $i = 0    ;
  $i < @twos         ; # <== doesn't get past this
  $i++
){ .say }
 
# this style of "loop" is not recommended anyway
 
# use one of the following instead
 
for @twos { .say } # a perfectly fine infinite loop
 
for @twos.kv -> $array-index, $result {
  say "2 ** $array-index == $result"
}
# ( gets to 2**8506 (2561 digits) in 30 seconds as of 2015-10-11 )
```