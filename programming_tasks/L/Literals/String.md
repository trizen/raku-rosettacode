[1]: http://rosettacode.org/wiki/Literals/String

# [Literals/String][1]

Unlike most languages that hardwire their quoting mechanisms, the quote mechanism in Perl 6 is extensible, and all normal-looking quotes actually derive from a parent quoting language called Q via grammatical mixins, applied via standard Perl 6 adverbial syntax.
The available quote mixins, straight from current spec S02, are:


#### Output:
```
Short       Long            Meaning
=====       ====            =======
:x          :exec           Execute as command and return results
:w          :words          Split result on words (no quote protection)
:ww         :quotewords     Split result on words (with quote protection)
:v          :val            Evaluate word or words for value literals
:q          :single         Interpolate \\, \q and \' (or whatever)
:qq         :double         Interpolate with :s, :a, :h, :f, :c, :b
:s          :scalar         Interpolate $ vars
:a          :array          Interpolate @ vars
:h          :hash           Interpolate % vars
:f          :function       Interpolate & calls
:c          :closure        Interpolate {...} expressions
:b          :backslash      Interpolate \n, \t, etc. (implies :q at least)
:to         :heredoc        Parse result as heredoc terminator
            :regex          Parse as regex
            :subst          Parse as substitution
            :trans          Parse as transliteration
            :code           Quasiquoting
:p          :path           Return a Path object (see S16 for more options
```


In any case, an initial <tt>Q</tt>, <tt>q</tt>, or <tt>qq</tt> may omit the initial colon to form traditional Perl quotes such as <tt>qw//</tt>.
And Q can be used by itself to introduce a quote that has no escapes at all except for the closing delimiter:

```perl
my $raw = Q'$@\@#)&!#';
```


Note that the single quotes there imply no single quoting semantics as they would in Perl 5. They're just the quotes the programmer happened to choose, since they were most like the raw quoting. Single quotes imply <tt>:q</tt> only when used as normal single quotes are, as discussed below.
As in Perl 5, you can use any non-alphanumeric, non-whitespace characters for delimiters with the general forms of quoting, including matching bracket characters, including any Unicode brackets.



Using the definitions above, we can derive the various standard "sugar" quotes from Q, including:


#### Output:
```
Normal  Means
======  =====
q/.../  Q :q /.../
qq/.../ Q :qq /.../
'...'   Q :q /.../
"..."   Q :qq /.../
<...>   Q :q :w :v /.../
«...»   Q :qq :ww :v /.../
/.../   Q :regex /.../
quasi {...} Q :code {...}
```


The <tt>:qq</tt>-derived languages all give normal Perlish interpolation, but individual interpolations may be chosen or suppressed with extra adverbs.



Unlike in Perl 5, we don't use backticks as shorthand for what is now expressed as <tt>qqx//</tt> in Perl 6.
(Backticks are now reserved for user-defined syntax.)
Heredocs now have no special <tt>&lt;&lt;</tt> syntax,
but fall out of the <tt>:to</tt> adverb:

```perl
say qq:to/END/;
    Your ad here.
    END
```


Indentation equivalent to the ending tag is automatically removed.



Backslash sequences recognized by <tt>:b</tt> (and hence <tt>:qq</tt>) include:

```perl
"\a"        # BELL
"\b"        # BACKSPACE
"\t"        # TAB
"\n"        # LINE FEED
"\f"        # FORM FEED
"\r"        # CARRIAGE RETURN
"\e"        # ESCAPE
"\x263a"    # ☺
"\o40"      # SPACE
"\0"        # NULL
 
"\cC"       # CTRL-C
"\c8"       # BACKSPACE
"\c[13,10]" # CRLF
"\c[LATIN CAPITAL LETTER A, COMBINING RING ABOVE]"
```


Leading <tt>0</tt> specifically does not mean octal in Perl 6;
you must use <tt>\o</tt> instead.