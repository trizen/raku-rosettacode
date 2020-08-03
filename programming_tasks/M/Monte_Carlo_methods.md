[1]: https://rosettacode.org/wiki/Monte_Carlo_methods

# [Monte Carlo methods][1]

We'll consider the upper-right quarter of the unitary disk centered at the origin. Its area is ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=1bced3de59030a7be8a5b3b67a2a691a&mode=mathml).

```raku
my @random_distances = ([+] rand**2 xx 2) xx *;
 
sub approximate_pi(Int $n) {
    4 * @random_distances[^$n].grep(* < 1) / $n
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

```raku
my @pi = ([\+] 4 * (1 > [+] rand**2 xx 2) xx *) Z/ 1 .. *;
say @pi[10, 1000, 10_000];
```