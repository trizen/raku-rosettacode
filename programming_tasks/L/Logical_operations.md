[1]: https://rosettacode.org/wiki/Logical_operations

# [Logical operations][1]

Perl 6 has an abundance of logical operators for various purposes.

```raku
sub logic($a,$b) {
    say "$a && $b is ", $a && $b;     # short-circuiting
    say "$a || $b is ", $a || $b;     # short-circuiting
    say "$a ^^ $b is ", $a ^^ $b;
    say "!$a is ",     !$a;
 
    say "$a ?& $b is ", $a ?& $b;     # non-short-circuiting
    say "$a ?| $b is ", $a ?| $b;     # non-short-circuiting
    say "$a ?^ $b is ", $a ?^ $b;     # non-short-circuiting
 
    say "$a +& $b is ", $a +& $b;     # numeric bitwise
    say "$a +| $b is ", $a +| $b;     # numeric bitwise
    say "$a +^ $b is ", $a +^ $b;     # numeric bitwise
 
    say "$a ~& $b is ", $a ~& $b;     # buffer bitwise
    say "$a ~| $b is ", $a ~| $b;     # buffer bitwise
    say "$a ~^ $b is ", $a ~| $b;     # buffer bitwise
 
    say "$a & $b is ", $a & $b;       # junctional/autothreading
    say "$a | $b is ", $a | $b;       # junctional/autothreading
    say "$a ^ $b is ", $a ^ $b;       # junctional/autothreading
 
    say "$a and $b is ", ($a and $b); # loose short-circuiting
    say "$a or $b is ",  ($a or $b);  # loose short-circuiting
    say "$a xor $b is ", ($a xor $b);
    say "not $a is ",    (not $a);
}
 
logic(3,10);
```

#### Output:
```
3 && 10 is 10
3 || 10 is 3
3 ^^ 10 is Nil
!3 is False
3 ?& 10 is True
3 ?| 10 is True
3 ?^ 10 is False
3 +& 10 is 2
3 +| 10 is 11
3 +^ 10 is 9
3 ~& 10 is 1
3 ~| 10 is 30
3 ~^ 10 is 30
3 & 10 is all(3, 10)
3 | 10 is any(3, 10)
3 ^ 10 is one(3, 10)
3 and 10 is 10
3 or 10 is 3
3 xor 10 is Nil
not 3 is False
```