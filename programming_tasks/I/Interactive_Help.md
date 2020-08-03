[1]: https://rosettacode.org/wiki/Interactive_Help

# [Interactive Help][1]

Perl 6 help is generally in a specialized text format known as POD (Plain Old Documentation). It is sometimes referred to as POD6 to distinguish it from Perl 5 POD which is slightly different and not completely compatible. Perl 6 has a local command line help app: p6doc. It also has online browsable HTML documentation at [docs.perl6.org](https://docs.perl6.org). If you want to download the HTML docs for a local copy, or just prefer to browse the documentation as a single page [docs.perl6.org/perl6.html](https://docs.perl6.org/perl6.html) may be more to your preference. If you prefer a different format, there are [utilities available](https://modules.perl6.org/search/?q=POD%3A%3ATo) to convert the POD docs to several different formats; Markdown, PDF, Latex, plain text, etc.



Individual Perl 6 scripts are to some extent self-documenting. If the script has a MAIN sub, and it is called with improper parameters, it will display an automatically generated help message showing the various possible parameters, which are required, which are optional, and what type each takes:

```perl
sub MAIN(
    Str $run,             #= Task or file name
    Str :$lang = 'perl6', #= Language, default perl6
    Int :$skip = 0,       #= Skip # to jump partially into a list
    Bool :f(:$force),     #= Override any skip parameter
) {
    # do whatever
}
```

#### Output:
```
Usage:
  main.p6 [--lang=<Str>] [--skip=<Int>] [-f|--force] <run>
  
    <run>           Task or file name
    --lang=<Str>    Language, default perl6
    --skip=<Int>    Skip # to jump partially into a list
    -f|--force      Override any skip parameter
```