[1]: https://rosettacode.org/wiki/Boolean_values

# [Boolean values][1]





Raku provides an enumeration `Bool` with two values, `True` and `False`. Values of enumerations can be used as ordinary values or as mixins:

```perl
my Bool $crashed = False;
my $val = 0 but True;
```


For a discussion of Boolean context (how Raku decides whether something is True or False): [https://docs.raku.org/language/contexts#index-entry-Boolean_context](https://docs.raku.org/language/contexts#index-entry-Boolean_context).
