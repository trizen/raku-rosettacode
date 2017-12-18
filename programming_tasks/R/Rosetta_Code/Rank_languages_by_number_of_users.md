[1]: http://rosettacode.org/wiki/Rosetta_Code/Rank_languages_by_number_of_users

# [Rosetta Code/Rank languages by number of users][1]

Use the mediawiki API rather than web scraping since it is much faster and less resource intensive. Show languages with more than 25 users since that is still a pretty short list and to demonstrate how tied rankings are handled. Change the **$minimum** parameter to adjust what the cut-off point will be.



This is all done in a single pass; ties are not detected until a language has the same count as a previous one, so ties are marked by a **T** next to the count indicating that **this** language has the same count as the **previous**.

```perl
use HTTP::UserAgent;
use JSON::Fast;
 
say "========= Generated: { DateTime.new(time) } =========";
my $start-time = now;
my $lang = 1;
my $rank = 0;
my $last = 0;
my $tie = ' ';
my $minimum = 25;
 
.say for
    mediawiki-query('http://rosettacode.org/mw', 'pages',
                    generator => 'categorymembers',
                    gcmtitle  => "Category:Language users",
                    prop      => 'categoryinfo')\
 
    .map({ %( count => .<categoryinfo><pages> || 0,
              lang  => .<title>.subst(/^'Category:' (.+) ' User'/, ->$/ {$0}) ) })\
 
    .sort( { -.<count>, .<lang> } )\
 
    .map( { last if .<count> < $minimum; display(.<count>, .<lang>) } );
 
say "========= elapsed: {(now - $start-time).round(.01)} seconds =========";
 
sub display ($count, $which) {
    if $last != $count { $last = $count; $rank = $lang; $tie = ' ' } else { $tie = 'T' };
    sprintf "#%3d  Rank: %2d %s  with %-4s users:  %s", $lang++, $rank, $tie, $count, $which;
}
 
sub mediawiki-query ($site, $type, *%query) {
    my $url = "$site/api.php?" ~ uri-query-string(
        :action<query>, :format<json>, :gcmlimit<350>, :rawcontinue(), |%query);
    my $continue = '';
    my $client = HTTP::UserAgent.new;
 
    gather loop {
        my $response = $client.get("$url&$continue");
 
        my $data = from-json($response.content);
        take $_ for $data.<query>.{$type}.values;
 
        $continue = uri-query-string |($data.<query-continue>{*}».hash.hash or last);
    }
}
 
sub uri-query-string (*%fields) {
    %fields.map({ "{.key}={uri-encode .value}" }).join("&")
}
 
sub uri-encode ($str) {
    $str.subst(/<[\x00..\xff]-[a..zA..Z0..9_.~-]>/, *.ord.fmt('%%%02X'), :g)
}
```

#### Output:
```
========= Generated: 2017-12-18T13:50:32Z =========
#  1  Rank:  1    with 373  users:  C
#  2  Rank:  2    with 261  users:  C++
#  3  Rank:  3    with 257  users:  Java
#  4  Rank:  4    with 243  users:  Python
#  5  Rank:  5    with 228  users:  JavaScript
#  6  Rank:  6    with 163  users:  PHP
#  7  Rank:  7    with 162  users:  Perl
#  8  Rank:  8    with 131  users:  SQL
#  9  Rank:  9    with 120  users:  UNIX Shell
# 10  Rank: 10    with 118  users:  BASIC
# 11  Rank: 11    with 113  users:  C sharp
# 12  Rank: 12    with 109  users:  Pascal
# 13  Rank: 13    with 98   users:  Haskell
# 14  Rank: 14    with 91   users:  Ruby
# 15  Rank: 15    with 71   users:  Fortran
# 16  Rank: 16    with 65   users:  Visual Basic
# 17  Rank: 17    with 60   users:  Scheme
# 18  Rank: 18    with 59   users:  Prolog
# 19  Rank: 19    with 57   users:  Common Lisp
# 20  Rank: 20    with 54   users:  Lua
# 21  Rank: 21    with 52   users:  AWK
# 22  Rank: 22    with 51   users:  HTML
# 23  Rank: 23    with 45   users:  Assembly
# 24  Rank: 24    with 44   users:  Batch File
# 25  Rank: 25    with 42   users:  X86 Assembly
# 26  Rank: 26    with 41   users:  Bash
# 27  Rank: 27    with 40   users:  Erlang
# 28  Rank: 28    with 37   users:  Forth
# 29  Rank: 29    with 35   users:  Lisp
# 30  Rank: 29 T  with 35   users:  MATLAB
# 31  Rank: 29 T  with 35   users:  Visual Basic .NET
# 32  Rank: 32    with 34   users:  J
# 33  Rank: 33    with 33   users:  Ada
# 34  Rank: 33 T  with 33   users:  Brainf***
# 35  Rank: 33 T  with 33   users:  Delphi
# 36  Rank: 33 T  with 33   users:  Objective-C
# 37  Rank: 37    with 32   users:  Tcl
# 38  Rank: 38    with 31   users:  APL
# 39  Rank: 38 T  with 31   users:  COBOL
# 40  Rank: 40    with 30   users:  R
# 41  Rank: 41    with 28   users:  Go
# 42  Rank: 41 T  with 28   users:  Perl 6
# 43  Rank: 43    with 27   users:  Clojure
# 44  Rank: 43 T  with 27   users:  Mathematica
# 45  Rank: 45    with 25   users:  AutoHotkey
========= elapsed: 1.81 seconds =========
```