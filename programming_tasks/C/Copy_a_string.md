[1]: http://rosettacode.org/wiki/Copy_a_string

# [Copy a string][1]

There is no special handling needed to copy a string; just assign it to a new variable:

```perl
my $original = 'Hello.';
my $copy = $original;
say $copy;            # prints "Hello."
$copy = 'Goodbye.';
say $copy;            # prints "Goodbye."
say $original;        # prints "Hello."
```


You can also bind a new variable to an existing one so that each refers to, and can modify the same string.

```perl
my $original = 'Hello.';
my $bound := $original;
say $bound;           # prints "Hello."
$bound = 'Goodbye.';
say $bound;           # prints "Goodbye."
say $original;        # prints "Goodbye."
```


You can also create a read-only binding which will allow read access to the string but prevent modification except through the original variable.

```perl
my $original = 'Hello.';
my $bound-ro ::= $original;
say $bound-ro;        # prints "Hello."
try {
  $bound-ro = 'Runtime error!';
  CATCH {
    say "$!";         # prints "Cannot modify readonly value"
  };
};
say $bound-ro;        # prints "Hello."
$original = 'Goodbye.';
say $bound-ro;        # prints "Goodbye."
```