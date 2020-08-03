[1]: https://rosettacode.org/wiki/Luhn_test_of_credit_card_numbers

# [Luhn test of credit card numbers][1]

Here we make use of `comb`, which splits into individual characters,
and the sequence operator `...`, which can intuit an even or odd sequence from the first two values.
`%%` is the divisible-by operator.

```perl
sub luhn-test ($number --> Bool) {
    my @digits = $number.comb.reverse;
    my $sum = @digits[0,2...*].sum
            + @digits[1,3...*].map({ |($_ * 2).comb }).sum;
    return $sum %% 10;
}
 
# And we can test it like this:
 
use Test;
 
my @cc-numbers =
    '49927398716'       => True,
    '49927398717'       => False,
    '1234567812345678'  => False,
    '1234567812345670'  => True;
 
plan @cc-numbers.elems;
 
for @cc-numbers».kv -> ($cc, $expected-result) {
    is luhn-test(+$cc), $expected-result,
        "$cc {$expected-result ?? 'passes' !! 'does not pass'} the Luhn test.";
}
```

#### Output:
```
1..4
ok 1 - 49927398716 passes the Luhn test.
ok 2 - 49927398717 does not pass the Luhn test.
ok 3 - 1234567812345678 does not pass the Luhn test.
ok 4 - 1234567812345670 passes the Luhn test.
```