[1]: http://rosettacode.org/wiki/Case-sensitivity_of_identifiers

# [Case-sensitivity of identifiers][1]

```perl
my $dog = 'Benjamin';
my $Dog = 'Samba';
my $DOG = 'Bernie';
say "The three dogs are named $dog, $Dog, and $DOG."
```


The only place that Perl�pays any attention to the case of identifiers is that, for certain error messages, it will guess that an identifier starting lowercase is probably a function name, while one starting uppercase is probably a type or constant name. But this case distinction is merely a convention in Perl, not mandatory:

```perl
constant dog = 'Benjamin';
sub Dog() { 'Samba' }
my &DOG = { 'Bernie' }
say "The three dogs are named {dog}, {Dog}, and {DOG}."
```