[1]: https://rosettacode.org/wiki/Rosetta_Code/Rank_languages_by_number_of_users

# [Rosetta Code/Rank languages by number of users][1]

Use the mediawiki API rather than web scraping since it is much faster and less resource intensive. Show languages with more than 25 users since that is still a pretty short list and to demonstrate how tied rankings are handled. Change the **$minimum** parameter to adjust what the cut-off point will be.



This is all done in a single pass; ties are not detected until a language has the same count as a previous one, so ties are marked by a **T** next to the count indicating that **this** language has the same count as the **previous**.

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
 
my $client = HTTP::UserAgent.new;
 
my $url = 'https://rosettacode.org/mw';
 
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
 
    .map({ %( count => .<categoryinfo><pages> || 0,
              lang  => .<title>.subst(/^'Category:' (.+) ' User'/, ->$/ {$0}) ) })
 
    .sort( { -.<count>, .<lang> } )
 
    .map( { last if .<count> < $minimum; display(.<count>, .<lang>) } );
 
say "========= elapsed: {(now - $start-time).round(.01)} seconds =========";
 
sub display ($count, $which) {
    if $last != $count { $last = $count; $rank = $lang; $tie = ' ' } else { $tie = 'T' };
    sprintf "#%3d  Rank: %2d %s  with %-4s users:  %s", $lang++, $rank, $tie, $count, $which;
}
 
sub mediawiki-query ($site, $type, *%query) {
    my $url = "$site/api.php?" ~ uri-query-string(
        :action<query>, :format<json>, :formatversion<2>, |%query);
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
========= Generated: 2018-06-01T22:09:26Z =========
#  1  Rank:  1    with 380  users:  C
#  2  Rank:  2    with 269  users:  Java
#  3  Rank:  3    with 266  users:  C++
#  4  Rank:  4    with 251  users:  Python
#  5  Rank:  5    with 234  users:  JavaScript
#  6  Rank:  6    with 167  users:  Perl
#  7  Rank:  7    with 166  users:  PHP
#  8  Rank:  8    with 134  users:  SQL
#  9  Rank:  9    with 125  users:  UNIX Shell
# 10  Rank: 10    with 119  users:  BASIC
# 11  Rank: 11    with 116  users:  C sharp
# 12  Rank: 12    with 112  users:  Pascal
# 13  Rank: 13    with 99   users:  Haskell
# 14  Rank: 14    with 93   users:  Ruby
# 15  Rank: 15    with 74   users:  Fortran
# 16  Rank: 16    with 67   users:  Visual Basic
# 17  Rank: 17    with 62   users:  Prolog
# 18  Rank: 18    with 61   users:  Scheme
# 19  Rank: 19    with 58   users:  Common Lisp
# 20  Rank: 20    with 55   users:  Lua
# 21  Rank: 21    with 53   users:  AWK
# 22  Rank: 22    with 52   users:  HTML
# 23  Rank: 23    with 46   users:  Assembly
# 24  Rank: 24    with 44   users:  Batch File
# 25  Rank: 25    with 42   users:  Bash
# 26  Rank: 25 T  with 42   users:  X86 Assembly
# 27  Rank: 27    with 40   users:  Erlang
# 28  Rank: 28    with 38   users:  Forth
# 29  Rank: 29    with 37   users:  MATLAB
# 30  Rank: 30    with 36   users:  Lisp
# 31  Rank: 31    with 35   users:  J
# 32  Rank: 31 T  with 35   users:  Visual Basic .NET
# 33  Rank: 33    with 34   users:  Delphi
# 34  Rank: 34    with 33   users:  APL
# 35  Rank: 34 T  with 33   users:  Ada
# 36  Rank: 34 T  with 33   users:  Brainf***
# 37  Rank: 34 T  with 33   users:  Objective-C
# 38  Rank: 34 T  with 33   users:  Tcl
# 39  Rank: 39    with 32   users:  R
# 40  Rank: 40    with 31   users:  COBOL
# 41  Rank: 41    with 30   users:  Go
# 42  Rank: 42    with 29   users:  Perl 6
# 43  Rank: 43    with 27   users:  Clojure
# 44  Rank: 43 T  with 27   users:  Mathematica
# 45  Rank: 45    with 25   users:  AutoHotkey
========= elapsed: 1.45 seconds =========
```