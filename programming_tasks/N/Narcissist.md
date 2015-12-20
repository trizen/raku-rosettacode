[1]: http://rosettacode.org/wiki/Narcissist

# [Narcissist][1]

For the narcissist to work you must be very careful with whitespace. The following version works if it is given to standard input as exactly one line terminated by a newline character.



Note how the code takes advantage of Perl 6's ability to nest quoting delimiters.

```perl6
EVAL my $self = q{say slurp() eq q[EVAL my $self = q{]~$self~q[}]~10.chr ?? q{Beautiful!} !! q{Not my type.}}
```

#### Output:
```
$ narcissist='EVAL my $self = q{say slurp() eq q[EVAL my $self = q{]~$self~q[}]~10.chr ?? q{Beautiful!} !! q{Not my type.}}'
$ perl6 -e "$narcissist" <<<"$narcissist"
Beautiful!
$ perl6 -e "$narcissist" <<<"$narcissist # a comment ruining it all" 
Not my type.
```