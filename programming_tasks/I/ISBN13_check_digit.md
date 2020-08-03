[1]: https://rosettacode.org/wiki/ISBN13_check_digit

# [ISBN13 check digit][1]

Also test a value that has a zero check digit.

```raku
sub check-digit ($isbn) {
     (10 - (sum (|$isbn.comb(/<[0..9]>/)) »*» (1,3)) % 10).substr: *-1
}
 
{
    my $check = .substr(*-1);
    my $check-digit = check-digit .chop;
    say "$_ : ", $check == $check-digit ??
        'Good' !!
        "Bad check-digit $check; should be $check-digit"
} for words <
    978-1734314502
    978-1734314509
    978-1788399081
    978-1788399083
    978-2-74839-908-0
    978-2-74839-908-5
>;
```

#### Output:
```
978-1734314502 : Good
978-1734314509 : Bad check-digit 9; should be 2
978-1788399081 : Good
978-1788399083 : Bad check-digit 3; should be 1
978-2-74839-908-0 : Good
978-2-74839-908-5 : Bad check-digit 5; should be 0
```
