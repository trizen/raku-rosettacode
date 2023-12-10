[1]: https://rosettacode.org/wiki/Rosetta_Code/Rank_languages_by_number_of_users

# [Rosetta Code/Rank languages by number of users][1]





Use the mediawiki API rather than web scraping since it is much faster and less resource intensive. Show languages with more than 25 users since that is still a pretty short list and to demonstrate how tied rankings are handled. Change the **$minimum** parameter to adjust what the cut-off point will be.



This is all done in a single pass; ties are not detected until a language has the same count as a previous one, so ties are marked by a **T** next to the count indicating that **this** language has the same count as the **previous**.

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;

my $client = HTTP::UserAgent.new;

my $url = 'https://rosettacode.org/w';

my $start-time = now;

say "========= Generated: { DateTime.new(time) } =========";

my $lang = 1;
my $rank = 0;
my $last = 0;
my $tie = ' ';
my $minimum = 25;

.say for
    mediawiki-query(
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle<Category:Language users>,
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<categoryinfo>
    )

    .map({ %( count => .<categoryinfo><pages> || 0,
              lang  => .<title>.subst(/^'Category:' (.+) ' User'/, ->$/ {$0}) ) })

    .sort( { -.<count>, .<syntaxhighlight lang="text"> } )

    .map( { last if .<count> < $minimum; display(.<count>, .<syntaxhighlight lang="text">) } );

say "========= elapsed: {(now - $start-time).round(.01)} seconds =========";

sub display ($count, $which) {
    if $last != $count { $last = $count; $rank = $lang; $tie = ' ' } else { $tie = 'T' };
    sprintf "#%3d  Rank: %2d %s  with %-4s users:  %s", $lang++, $rank, $tie, $count, $which;
}

sub mediawiki-query ($site, $type, *%query) {
    my $url = "$site/api.php?" ~ uri-query-string(
        :action<query>, :format<json>, :formatversion<2>, |%query);
    my $continue = '';

    gather loop {
        my $response = $client.get("$url&$continue");
        my $data = from-json($response.content);
        take $_ for $data.<query>.{$type}.values;
        $continue = uri-query-string |($data.<query-continue>{*}».hash.hash or last);
    }
}

sub uri-query-string (*%fields) {
    join '&', %fields.map: { "{.key}={uri-escape .value}" }
}
```

#### Output:
```
========= Generated: 2022-09-02T20:59:20Z =========
#  1  Rank:  1    with 441  users:  C
#  2  Rank:  2    with 321  users:  Java
#  3  Rank:  3    with 319  users:  Python
#  4  Rank:  4    with 314  users:  C++
#  5  Rank:  5    with 291  users:  JavaScript
#  6  Rank:  6    with 190  users:  PHP
#  7  Rank:  7    with 186  users:  Perl
#  8  Rank:  8    with 167  users:  SQL
#  9  Rank:  9    with 151  users:  UNIX Shell
# 10  Rank: 10    with 133  users:  Pascal
# 11  Rank: 11    with 132  users:  BASIC
# 12  Rank: 11 T  with 132  users:  C sharp
# 13  Rank: 13    with 115  users:  Haskell
# 14  Rank: 14    with 107  users:  Ruby
# 15  Rank: 15    with 94   users:  Fortran
# 16  Rank: 16    with 75   users:  Visual Basic
# 17  Rank: 17    with 72   users:  Scheme
# 18  Rank: 18    with 70   users:  Prolog
# 19  Rank: 19    with 68   users:  AWK
# 20  Rank: 19 T  with 68   users:  Lua
# 21  Rank: 21    with 66   users:  Common Lisp
# 22  Rank: 22    with 61   users:  HTML
# 23  Rank: 23    with 55   users:  X86 Assembly
# 24  Rank: 24    with 50   users:  Forth
# 25  Rank: 25    with 49   users:  Batch File
# 26  Rank: 26    with 47   users:  Assembly
# 27  Rank: 27    with 45   users:  Bash
# 28  Rank: 27 T  with 45   users:  MATLAB
# 29  Rank: 29    with 42   users:  Lisp
# 30  Rank: 30    with 41   users:  APL
# 31  Rank: 30 T  with 41   users:  Erlang
# 32  Rank: 32    with 40   users:  Delphi
# 33  Rank: 32 T  with 40   users:  R
# 34  Rank: 34    with 39   users:  J
# 35  Rank: 34 T  with 39   users:  Visual Basic .NET
# 36  Rank: 36    with 38   users:  Go
# 37  Rank: 36 T  with 38   users:  Tcl
# 38  Rank: 38    with 37   users:  COBOL
# 39  Rank: 39    with 36   users:  Smalltalk
# 40  Rank: 40    with 35   users:  Brainf***
# 41  Rank: 40 T  with 35   users:  Objective-C
# 42  Rank: 42    with 34   users:  Clojure
# 43  Rank: 43    with 33   users:  Mathematica
# 44  Rank: 44    with 30   users:  OCaml
# 45  Rank: 45    with 28   users:  LaTeX
# 46  Rank: 46    with 27   users:  AutoHotkey
# 47  Rank: 46 T  with 27   users:  Emacs Lisp
# 48  Rank: 46 T  with 27   users:  PostScript
# 49  Rank: 46 T  with 27   users:  REXX
# 50  Rank: 50    with 26   users:  Perl 6
# 51  Rank: 50 T  with 26   users:  Sed
# 52  Rank: 52    with 25   users:  CSS
# 53  Rank: 52 T  with 25   users:  Scala
# 54  Rank: 52 T  with 25   users:  VBScript
========= elapsed: 1.48 seconds =========

========= elapsed: 1.45 seconds =========
```
