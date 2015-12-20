[1]: http://rosettacode.org/wiki/Matrix_multiplication

# [Matrix multiplication][1]

There are three ways in which this example differs significantly from the original Perl&#160;5 code. These are not esoteric differences; all three of these features typically find heavy use in Perl&#160;6.



First, we can use a real signature that can bind two arrays as arguments, because the default in Perl&#160;6 is not to flatten arguments unless the signature specifically requests it.
We don't need to pass the arrays with backslashes because the binding choice is made lazily
by the signature itself at run time; in Perl&#160;5 this choice must be made at compile time.
Also, we can bind the arrays to formal parameters that are really lexical variable names; in Perl&#160;5 they can only be bound to global array objects (via a typeglob assignment).



Second, we use the X cross operator in conjunction with a two-parameter closure to avoid writing
nested loops. The X cross operator, along with Z, the zip operator, is a member of a class of operators that expect lists on both sides, so we call them "list infix" operators. We tend to define these operators using capital letters so that they stand out visually from the lists on both sides. The cross operator makes every possible combination of the one value from the first list followed by one value from the second. The right side varies most rapidly, just like an inner loop. (The X and Z operators may both also be used as meta-operators, Xop or Zop, distributing some other operator "op" over their generated list. All metaoperators in Perl&#160;6 may be applied to user-defined operators as well.)



Third is the use of prefix `^` to generate a list of numbers in a range. Here it is
used on an array to generate all the indexes of the array. We have a way of indicating a range by the infix `..` operator, and you can put a `^` on either end to exclude that endpoint. We found ourselves writing `0 ..^ @a` so often that we made `^@a` a shorthand for that. It's pronounced "upto". The array is evaluated in a numeric context, so it returns the number of elements it contains, which is exactly what you want for the exclusive limit of the range.

```perl
sub mmult(@a,@b) {
    my @p;
    for ^@a X ^@b[0] -> ($r, $c) {
        @p[$r][$c] += @a[$r][$_] * @b[$_][$c] for ^@b;
    }
    @p;
}
 
my @a = [1,  1,  1,   1],
        [2,  4,  8,  16],
        [3,  9, 27,  81],
        [4, 16, 64, 256];
 
my @b = [  4  , -3  ,  4/3,  -1/4 ],
        [-13/3, 19/4, -7/3,  11/24],
        [  3/2, -2  ,  7/6,  -1/4 ],
        [ -1/6,  1/4, -1/6,   1/24];
 
.say for mmult(@a,@b);
```

#### Output:
```
[1 0 0 0]
[0 1 0 0]
[0 0 1 0]
[0 0 0 1]
```


Note that these are not rounded values, but exact, since all the math was done in rationals.
Hence we need not rely on format tricks to hide floating-point inaccuracies.



Just for the fun of it, here's a functional version that uses no temp variables or side effects.
Some people will find this more readable and elegant, and others will, well, not.

```perl
sub mmult(\a,\b) {
    [
        for ^a -> \r {
            [
                for ^b[0] -> \c {
                    [+] a[r;^b] Z* b[^b;c]
                }
            ]
        }
    ]
}
```


Here we use Z with an "op" of `*`, which is a zip with multiply. This, along with the `[+]` reduction operator, replaces the inner loop. We chose to split the outer X loop back into two loops to make it convenient to collect each subarray value in `[...]`. It just collects all the returned values from the inner loop and makes an array of them. The outer loop simply returns the outer array.