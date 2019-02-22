[1]: https://rosettacode.org/wiki/Unit_testing

# [Unit testing][1]

Perl 6 does not really have a mechanism built into the compiler to do unit testing, (It **does** have design-by-contract capabilities built in, but that is more for run-time, not really for unit testing.) Perl 6, and Perl in general **does** have unit testing built into the community. The [Test-Anything-Protocol](http://en.wikipedia.org/wiki/Test_Anything_Protocol) was invented and developed specifically to do unit testing on the original version of Perl, is still heavily used for modern versions, and has become a major standard for unit testing in many different languages other than Perl.



Testing is such a basic value in Perl 6, that a suite of unit tests **is** the language specification. As decreed by Larry Wall, the original author of Perl and the lead designer of Perl 6, anything that can pass "roast", [the official Perl 6 test suite](https://github.com/perl6/roast), or at least majority portions of it, can call itself Perl 6.



Unit testing tools are not built in to the base compiler. Basic tools are distributed with the compiler as loadable modules. Many more complex toolkits and test harnesses are available through the [Perl 6 ecosystem](https://modules.perl6.org/). In general, it is uncommon to include testing code in with run-time code; not unheard-of and not forbidden, but uncommon. Instead Perl modules by convention and community pressure tend to have a test suite that is automatically run when the module is installed. Again, there is no rule that a module that is released for public consumption **must** have a test suite, but the community tends to favour modules that have them and discourage/avoid those without.