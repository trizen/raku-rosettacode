[1]: https://rosettacode.org/wiki/Maximum_difference_between_adjacent_elements_of_list

# [Maximum difference between adjacent elements of list][1]

```perl
sub max-diff (*@list) {
   return 0 if +@list < 2;
   my $max = @list.rotor(2 => -1).max({ (.[0] - .[1]).abs }).map( (* - *).abs )[0];
   "$max @ elements { @list.rotor(2 => -1).grep( { (.[0] - .[1]).abs == $max } ).gist }"
}
 
 
sub min-diff (*@list) {
   return 0 if +@list < 2;
   my $min = @list.rotor(2 => -1).min({ (.[0] - .[1]).abs }).map( (* - *).abs )[0];
   "$min @ elements { @list.rotor(2 => -1).grep( { (.[0] - .[1]).abs == $min } ).gist }"
}
 
 
sub avg-diff (*@list) { (+@list > 1) ?? (@list.sum / +@list) !! 0 }
 
 
# TESTING
 
for (
      [ 1,8,2,-3,0,1,1,-2.3,0,5.5,8,6,2,9,11,10,3 ]
     ,[(rand × 1e6) xx 6]
     ,[ e, i, τ, π, ∞ ]
     ,[ 1.9+3.7i, 2.07-13.2i, 0.2+-2.2i, 4.6+0i ]
     ,[ 6 ]
     ,[]
     ,[<One Two Three>]
    )
-> @list {
    say 'List: ', ~ @list.raku;
    for ('  Maximum', &max-diff ,'  Minimum', &min-diff, '  Average', &avg-diff)
    -> $which, &sub {
        say "$which distance between list elements: " ~ &sub(@list);
    }
    say '';
}
```

#### Output:
```
List: [1, 8, 2, -3, 0, 1, 1, -2.3, 0, 5.5, 8, 6, 2, 9, 11, 10, 3]
  Maximum distance between list elements: 7 @ elements ((1 8) (2 9) (10 3))
  Minimum distance between list elements: 0 @ elements ((1 1))
  Average distance between list elements: 3.658824

List: [480470.3324944039e0, 719954.3173377671e0, 648221.5674277907e0, 340053.78585537826e0, 540629.6741075241e0, 4336.700602958543e0]
  Maximum distance between list elements: 536292.9735045655 @ elements ((540629.6741075241 4336.700602958543))
  Minimum distance between list elements: 71732.74990997638 @ elements ((719954.3173377671 648221.5674277907))
  Average distance between list elements: 455611.06297097047

List: [2.718281828459045e0, <0+1i>, 6.283185307179586e0, 3.141592653589793e0, Inf]
  Maximum distance between list elements: Inf @ elements ((3.141592653589793 Inf))
  Minimum distance between list elements: 2.896386731590008 @ elements ((2.718281828459045 0+1i))
  Average distance between list elements: Inf+0.2i

List: [<1.9+3.7i>, <2.07-13.2i>, <0.2-2.2i>, <4.6+0i>]
  Maximum distance between list elements: 16.900855007957436 @ elements ((1.9+3.7i 2.07-13.2i))
  Minimum distance between list elements: 4.919349550499537 @ elements ((0.2-2.2i 4.6+0i))
  Average distance between list elements: 2.1925-2.925i

List: [6]
  Maximum distance between list elements: 0
  Minimum distance between list elements: 0
  Average distance between list elements: 0

List: []
  Maximum distance between list elements: 0
  Minimum distance between list elements: 0
  Average distance between list elements: 0

List: ["One", "Two", "Three"]
Cannot convert string to number: base-10 number must begin with valid digits or '.' in '⏏Two' (indicated by ⏏)
  in sub max-diff at listdiff.p6 line 3
  in block  at listdiff.p6 line 33
  in block <unit> at listdiff.p6 line 20
```
