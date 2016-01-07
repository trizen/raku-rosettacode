[1]: http://rosettacode.org/wiki/Mutual_recursion

# [Mutual recursion][1]

A direct translation of the definitions of <img class="mwe-math-fallback-image-inline tex" alt="F" src="http://rosettacode.org/mw/images/math/8/0/0/800618943025315f869e4e1f09471012.png"/> and <img class="mwe-math-fallback-image-inline tex" alt="M" src="http://rosettacode.org/mw/images/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png"/>:

```perl
multi F(0) { 1 }; multi M(0) { 0 }
multi F(\𝑛) { 𝑛 - M(F(𝑛 - 1)) }
multi M(\𝑛) { 𝑛 - F(M(𝑛 - 1)) }
 
say map &F, ^20;
say map &M, ^20;
```

#### Output:
```
1 1 2 2 3 3 4 5 5 6 6 7 8 8 9 9 10 11 11 12
0 0 1 2 2 3 4 4 5 6 6 7 7 8 9 9 10 11 11 12
```