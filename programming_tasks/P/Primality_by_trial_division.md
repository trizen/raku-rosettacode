[1]: http://rosettacode.org/wiki/Primality_by_trial_division

# [Primality by trial division][1]

Here we use a "none" junction which will autothread through the <tt>%%</tt> "is divisible by" operator to assert that <tt>$i</tt> is not divisible by 2 or any of the odd numbers up to its square root. Read it just as you would English: "Integer <tt>$i</tt> is prime if it is greater than one and is divisible by none of 2, 3, whatever + 2, up to but not including whatever is greater than the square root of <tt>$i</tt>."

```perl
sub prime (Int $i --> Bool) {
    $i > 1 and $i %% none 2, 3, *+2 ...^ * >= sqrt $i;
}
```


(No pun indented.)



This can easily be improved in two ways. First, we generate the primes so we only divide by those, instead of all odd numbers. Second, we memoize the result using the <tt>//=</tt> idiom of Perl, which does the right-hand calculation and assigns it only if the left side is undefined. We avoid recalculating the square root each time. Note the mutual recursion that depends on the implicit laziness of list evaluation:

```perl
my @primes = 2, 3, 5, -> $p { ($p+2, $p+4 ... &prime)[*-1] } ... *;
my @isprime = False,False;   # 0 and 1 are not prime by definition
sub prime($i) {
    my \limit = $i.sqrt.floor;
    @isprime[$i] //= so ($i %% none @primes ...^ { $_ > limit })
}
 
say "$_ is{ "n't" x !prime($_) } prime." for 1 .. 100;
```