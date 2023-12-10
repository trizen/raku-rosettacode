[1]: https://rosettacode.org/wiki/Documentation

# [Documentation][1]


Similarly to Perl 5, Raku is documented using [Pod](https://docs.raku.org/language/pod) (a redesigned version of POD). However, it's not simply ignored by the parser as in Perl 5, it's an internal part of the language spec and can be used to attach documentation objects to almost any part of the code.

```perl
#= it's yellow
sub marine { ... }
say &marine.WHY; # "it's yellow"

#= a shaggy little thing
class Sheep {
    #= not too scary
    method roar { 'roar!' }
}
say Sheep.WHY; # "a shaggy little thing"
say Sheep.^find_method('roar').WHY; # "not too scary"
```


A compiler has a built-in `--doc` switch that will generate documentation from a given source file. An output format can be specified in the switch. For example, `--doc=Markdown` will generate Markdown from Pod provided that the `Pod::To::Markdown` is installed.
