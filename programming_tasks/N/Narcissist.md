[1]: https://rosettacode.org/wiki/Narcissist

# [Narcissist][1]





For the narcissist to work you must be very careful with whitespace. The following version works if it is given to standard input as exactly one line terminated by a newline character.



Note how the code takes advantage of Raku's ability to nest quoting delimiters.

```perl
EVAL my $self = q{say slurp() eq q[EVAL my $self = q{]~$self~q[}]~10.chr ?? q{Beautiful!} !! q{Not my type.}}
```

#### Output:
```
$ narcissist='EVAL my $self = q{say slurp() eq q[EVAL my $self = q{]~$self~q[}]~10.chr ?? q{Beautiful!} !! q{Not my type.}}'
$ raku -e "$narcissist" <<<"$narcissist"
Beautiful!
$ raku -e "$narcissist" <<<"$narcissist # a comment ruining it all" 
Not my type.
```
