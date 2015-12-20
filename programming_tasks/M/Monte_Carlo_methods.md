[1]: http://rosettacode.org/wiki/Monte_Carlo_methods

# [Monte Carlo methods][1]

We'll consider the upper-right quarter of the unitary disk centered at the origin. Its area is <img class="tex" alt="\pi \over 4" src="/mw/images/math/e/9/9/e996309bbdde3b8457909580d174720e.png"/>.

```perl
my @random_distances = ([+] [rand](http://perldoc.perl.org/functions/rand.html)**2 xx 2) xx *;
 
sub approximate_pi(Int $n) {
    4 * @random_distances[^$n].[grep](http://perldoc.perl.org/functions/grep.html)(* < 1) / $n
}
 
say "Monte-Carlo π approximation:";
say "$_ iterations:  ", approximate_pi $_
    for 100, 1_000, 10_000;
 
```

#### Output:
```
Monte-Carlo π approximation:
100 iterations:  2.88
1000 iterations:  3.096
10000 iterations:  3.1168
```


We don't really need to write a function, though. A lazy list would do:

```perl6
my @pi = ([\+] 4 * (1 > [+] rand**2 xx 2) xx *) Z/ 1 .. *;
say @pi[10, 1000, 10_000];
```