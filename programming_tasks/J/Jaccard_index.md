[1]: https://rosettacode.org/wiki/Jaccard_index

# [Jaccard index][1]

```perl
sub J(\A, \B) { A ∪ B ?? (A ∩ B) / (A ∪ B) !! A ∪ B == A ∩ B ?? 1 !! 0 }
 
my %p =
  A => < >,
  B => <1 2 3 4 5>,
  C => <1 3 5 7 9>,
  D => <2 4 6 8 10>,
  E => <2 3 5 7>,
  F => <8>,
;
 
.say for %p.sort;
say '';
say "J({.join: ','}) = ", J |%p{$_} for [X] <A B C D E F> xx 2;
```

#### Output:
```
A => ()
B => (1 2 3 4 5)
C => (1 3 5 7 9)
D => (2 4 6 8 10)
E => (2 3 5 7)
F => 8

J(A,A) = 1
J(A,B) = 0
J(A,C) = 0
J(A,D) = 0
J(A,E) = 0
J(A,F) = 0
J(B,A) = 0
J(B,B) = 1
J(B,C) = 0.428571
J(B,D) = 0.25
J(B,E) = 0.5
J(B,F) = 0
J(C,A) = 0
J(C,B) = 0.428571
J(C,C) = 1
J(C,D) = 0
J(C,E) = 0.5
J(C,F) = 0
J(D,A) = 0
J(D,B) = 0.25
J(D,C) = 0
J(D,D) = 1
J(D,E) = 0.125
J(D,F) = 0.2
J(E,A) = 0
J(E,B) = 0.5
J(E,C) = 0.5
J(E,D) = 0.125
J(E,E) = 1
J(E,F) = 0
J(F,A) = 0
J(F,B) = 0
J(F,C) = 0
J(F,D) = 0.2
J(F,E) = 0
J(F,F) = 1
```
