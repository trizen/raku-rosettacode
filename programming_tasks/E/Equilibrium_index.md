[1]: http://rosettacode.org/wiki/Equilibrium_index

# [Equilibrium index][1]

```perl
sub equilibrium_index(@list) {
    my ($left,$right) = 0, [+] @list;
 
    gather for @list.kv -> $i, $x {
        $right -= $x;
        take $i if $left == $right;
        $left += $x;
    }
}
 
my @list = -7, 1, 5, 2, -4, 3, 0;
.say for equilibrium_index(@list).grep(/\d/);
```


And here's an FP solution that manages to remain O(n):

```perl
sub equilibrium_index(@list) {
    my @a = [\+] @list;
    my @b := reverse [\+] reverse @list;
    ^@list Zxx (@a »==« @b); 
}
```


The `[\+]` is a reduction that returns a list of partial results. The `»==«` is a vectorized equality comparison; it returns a vector of true and false. The `Zxx` is a zip with the list replication operator, so we return only the elements of the left list where the right list is true (which is taken to mean 1 here). And the `^@list` is just shorthand for `0 ..^ @list`. We could just as easily have used `@list.keys` there.



The task can be restated in a way that removes the "right side" from the calculation.



C is the current element,

L is the sum of elements left of C,

R is the sum of elements right of C,

S is the sum of the entire list.



By definition, L + C + R == S for any choice of C, and L == R for any C that is an equilibrium point.

Therefore (by substituting L for R), L + C + L == S at all equilibrium points.

Restated, 2L + C == S.

```perl
# Original example, with expanded calculations:
    0    1    2    3    4    5    6   # Index
   -7    1    5    2   -4    3    0   # C (Value at index)
    0   -7   -6   -1    1   -3    0   # L (Sum of left)
   -7  -13   -7    0   -2   -3    0   # 2L+C
```


If we build a hash as we walk the list, with 2L+C as hash keys, and arrays of C-indexes as hash values, we get:

```perl
{
     -7 => [ 0, 2 ],
    -13 => [ 1    ],
      0 => [ 3, 6 ],
     -2 => [ 4    ],
     -3 => [ 5    ],
}
```


After we have finished walking the list, we will have the sum (S), which we look up in the hash. Here S=0, so the equilibrium points are 3 and 6.



Note: In the code below, it is more convenient to calculate 2L+C \*after\* L has already been incremented by C; the calculation is simply 2L-C, because each L has an extra C in it. 2(L-C)+C == 2L-C.

```perl
sub eq_index ( *@list ) {
    my $sum = 0;
 
    my %h = @list.keys.classify: {
        $sum += @list[$_];
        $sum * 2 - @list[$_];
    };
 
    return %h{$sum} // [];
}
 
say eq_index < -7  1  5  2 -4  3  0 >; # 3 6
say eq_index <  2  4  6             >; # (no eq point)
say eq_index <  2  9  2             >; # 1
say eq_index <  1 -1  1 -1  1 -1  1 >; # 0 1 2 3 4 5 6
```


The `.classify` method creates a hash, with its code block's return value as key. Each hash value is an Array of all the inputs that returned that key.



We could have used `.pairs` instead of `.keys` to save the cost of `@list` lookups, but that would change each `%h` value to an Array of Pairs, which would complicate the return line.