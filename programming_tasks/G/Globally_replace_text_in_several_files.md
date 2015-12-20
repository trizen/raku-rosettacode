[1]: http://rosettacode.org/wiki/Globally_replace_text_in_several_files

# [Globally replace text in several files][1]

Current Perl 6 implementations do not yet support the -i flag for editing files in place, so we roll our own (rather unsafe) version:

```perl6
slurp($_).subst('Goodbye London!', 'Hello New York!', :g) ==> spurt($_)
    for <a.txt b.txt c.txt>;
```