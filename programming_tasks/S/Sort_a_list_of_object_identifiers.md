[1]: https://rosettacode.org/wiki/Sort_a_list_of_object_identifiers

# [Sort a list of object identifiers][1]

The `sort` routine accepts a sort key callback as the first argument. Here we generate a list of integers as the sort key for each OID, which gets sorted lexicographically with numeric comparison by default.

```raku
.say for sort *.comb(/\d+/)».Int, <
    1.3.6.1.4.1.11.2.17.19.3.4.0.10
    1.3.6.1.4.1.11.2.17.5.2.0.79
    1.3.6.1.4.1.11.2.17.19.3.4.0.4
    1.3.6.1.4.1.11150.3.4.0.1
    1.3.6.1.4.1.11.2.17.19.3.4.0.1
    1.3.6.1.4.1.11150.3.4.0
>;
```

#### Output:
```
1.3.6.1.4.1.11.2.17.5.2.0.79
1.3.6.1.4.1.11.2.17.19.3.4.0.1
1.3.6.1.4.1.11.2.17.19.3.4.0.4
1.3.6.1.4.1.11.2.17.19.3.4.0.10
1.3.6.1.4.1.11150.3.4.0
1.3.6.1.4.1.11150.3.4.0.1
```


Alternatively, using the `sprintf`-based approach used by the Perl solution, for comparison *(input elided)*:

```raku
.say for sort *.split('.').fmt('%08d'), <...>;
```


Or if using a third-party module is acceptable:

```raku
use Sort::Naturally;
 
.say for sort &naturally, <...>;
```