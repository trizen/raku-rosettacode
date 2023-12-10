[1]: https://rosettacode.org/wiki/Metaprogramming

# [Metaprogramming][1]





Raku makes it very easy to do metaprogramming. It is a basic goal of the language.



It is trivial to add a new operator. Most Raku operators are written as normal multiple-dispatch functions in the setting (known as a "prelude" in other languages, but in Raku the setting is a lexical scope notionally surrounding your compilation unit).



There is no a built in factorial operator Raku. It was purposely left out to use as a demonstration of how easy it is to add it. `:-)`

```perl
sub postfix:<!> { [*] 1..$^n }
say 5!;  # prints 120
```


You may augment a base class with a new method, as long as you declare that you are going to cheat.



Here we add a new method to do natural sorting to the base class `Any`. (`List` and `Array` are both subclasses of Any)

```perl
use MONKEY-TYPING; # Needed to do runtime augmentation of a base class.

augment class List {
    method nsort { self.list.sort: {$^a.lc.subst(/(\d+)/, -> $/ {0 ~ $0.chars.chr ~ $0 }, :g) ~ "\x0" ~ $^a} }
};

say ~<a201st a32nd a3rd a144th a17th a2 a95>.nsort;
say ~<a201st a32nd a3rd a144th a17th a2 a95>.sort;
```


Prints


```
a2 a3rd a17th a32nd a95 a144th a201st
a144th a17th a2 a201st a32nd a3rd a95
```


Grammar mixins work in Raku because grammar rules are just methods in grammar classes, and Raku automatically writes a JIT lexer for you whenever you derive a new language.  This functionality already works internally in the standard parser—what is not yet implemented is the `augment slang` hook to allow user code to do this mixin.  Raku itself is already parsed using such grammar mixins to provide extensible quoting and regex syntax.  For example, every time you pick your own quote characters, you're actually deriving a new Raku dialect that supports those start and stop characters.  Likewise any switches to impose single or double-quote semantics, or heredoc semantics, is merely a grammar mixin on the basic `Q` language.

```perl
say "Foo = $foo\n";  # normal double quotes
say Q:qq 【Foo = $foo\n】; # a more explicit derivation, new quotes
```
