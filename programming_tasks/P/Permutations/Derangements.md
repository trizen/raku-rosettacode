[1]: https://rosettacode.org/wiki/Permutations/Derangements

# [Permutations/Derangements][1]

Generate `List.permutations` and keep the ones where no elements are in their original position. This is done by zipping each permutation with the original list, and keeping the ones where none of the zipped pairs are equal.



I am using the `Z` infix zip operator with the `eqv` equivalence infix operator, all wrapped inside a `none()` Junction.



Although not necessary for this task, I have used `eqv` instead of `==` so that the `derangements()` function also works with any set of arbitrary objects (eg. strings, lists, etc.)

```raku
sub derangements(@l) {
    @l.permutations.grep(-> @p { none(@p Zeqv @l) })
}
 
sub prefix:<!>(Int $n) {
    (1, 0, 1, -> $a, $b { ($++ + 2) × ($b + $a) } ... *)[$n]
}
 
say 'derangements([1, 2, 3, 4])';
say derangements([1, 2, 3, 4]), "\n";
 
say 'n == !n == derangements(^n).elems';
for 0 .. 9 -> $n {
    say "!$n == { !$n } == { derangements(^$n).elems }"
}
```

#### Output:
```
derangements([1, 2, 3, 4])
((2 1 4 3) (2 3 4 1) (2 4 1 3) (3 1 4 2) (3 4 1 2) (3 4 2 1) (4 1 2 3) (4 3 1 2) (4 3 2 1))

n == !n == derangements(^n).elems
!0 == 1 == 1
!1 == 0 == 0
!2 == 1 == 1
!3 == 2 == 2
!4 == 9 == 9
!5 == 44 == 44
!6 == 265 == 265
!7 == 1854 == 1854
!8 == 14833 == 14833
!9 == 133496 == 133496
```