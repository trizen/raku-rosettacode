[1]: http://rosettacode.org/wiki/Pragmatic_directives

# [Pragmatic directives][1]

The Perl 6 pragma mechanism is nearly identical to Perl 5's, piggybacking on the notation for importing modules (pragmas are distinguished by case from normal modules, which are generally of mixed case). By convention pragmas are lowercase, unless they are indicating the use of an unsafe feature, in which case they are in all caps.

```perl
use MONKEY-TYPING;
augment class Int {
    method times (&what) { what() xx self }  # pretend like we're Ruby
}
```


Unlike Perl 5, there is no <tt>use strict;</tt> pragma, however, since Perl 6 is strict by default. Importation of a pragma is lexically scoped as in Perl 5, but note that unlike in Perl 5, _all_ importation is lexical in Perl 6, so pragmas are not special that way.