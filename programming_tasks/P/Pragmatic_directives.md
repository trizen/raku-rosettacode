[1]: https://rosettacode.org/wiki/Pragmatic_directives

# [Pragmatic directives][1]





The Perl&#160;6 pragma mechanism is nearly identical to Perl&#160;5's, piggybacking on the notation for importing modules (pragmas are distinguished by case from normal modules, which are generally of mixed case).  By convention pragmas are lowercase, unless they are indicating the use of an unsafe feature, in which case they are in all caps.

```perl
use MONKEY-TYPING;
augment class Int {
    method times (&what) { what() xx self }  # pretend like we're Ruby
}
```


Unlike Perl 5, there is no `use strict;` pragma, however, since Perl&#160;6 is strict by default.  Importation of a pragma is lexically scoped as in Perl&#160;5, but note that unlike in Perl&#160;5, *all* importation is lexical in Perl&#160;6, so pragmas are not special that way.
