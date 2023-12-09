[1]: https://rosettacode.org/wiki/Decorate-sort-undecorate_idiom

# [Decorate-sort-undecorate idiom][1]

It is somewhat rare to do, or even need to do an explicit schwartzian transform in Raku. You can pass a transform function to the sort operator, and it will use it to do its comparisons. As long as the transform is arity one (only takes one value,) the sort will *automatically* perform a schwartzian transform transparently, behind the scenes.



Here the transform `.chars` is arity one, so a schwartzian transform is performed automatically by the compiler.

```perl
# automatic schwartzian transform
dd <Rosetta Code is a programming chrestomathy site>.sort: *.chars;

# explicit schwartzian transform
dd <Rosetta Code is a programming chrestomathy site>.map({$_=>.chars}).sort({$^one.value cmp $^the-other.value}).map({.key});
```


( `dd` is the built in "data-dumper" function; a verbose and explicit representation of an objects contents.)


#### Output:
```
("a", "is", "Code", "site", "Rosetta", "programming", "chrestomathy").Seq
("a", "is", "Code", "site", "Rosetta", "programming", "chrestomathy").Seq
```


More complicated transforms may *require* an explicit schwartzian transform routine. An example of where an explicit transform is desirable is the `schwartzian()` routine in the [Raku entry for the P-value_correction task](https://rosettacode.org/wiki/P-value_correction#Raku).
