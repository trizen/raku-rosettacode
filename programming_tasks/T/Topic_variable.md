[1]: https://rosettacode.org/wiki/Topic_variable

# [Topic variable][1]

As in previous versions of Perl, in Perl6 the topic variable is $\_. In addition to a direct affectation, it can also be set with the 'given' keyword. A method can be called from it with an implicit call:

```raku
given 3 {
    .say for $_**2, .sqrt;
}
```


The scope of the `$_` variable is always lexical in Perl 6, though a function can access its caller's topic if it asks for it specially via `CALLER::<$_>`.