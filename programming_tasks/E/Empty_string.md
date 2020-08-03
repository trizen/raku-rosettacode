[1]: https://rosettacode.org/wiki/Empty_string

# [Empty string][1]

```raku
my $s = '';
say 'String is empty' unless $s;
say 'String is not empty' if $s;
```


Unlike in Perl 5, only empty strings test as false - the string "0" tests as true now.