[1]: https://rosettacode.org/wiki/Rosetta_Code/Rank_languages_by_popularity

# [Rosetta Code/Rank languages by popularity][1]

### Perl 6: Using the API



Note that this counts **only** the tasks. It does not include other non-task categories in the counts yielding more realistic, non-inflated numbers. Perl 6 is unicode aware and handles non-ASCII names natively. This does not attempt to 'unify' different language names that are the same behind the scenes as a result of Rosettacodes' capitalization peculiarities. (E.G. μC++, UC++ &amp; ΜC++)

```raku
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
use Sort::Naturally;
 
my $client = HTTP::UserAgent.new;
 
my $url = 'http://rosettacode.org/mw';
 
my $tablefile = './RC_Popularity.txt';
 
my %counts =
    mediawiki-query(
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle<Category:Programming Languages>,
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<categoryinfo>
    )
    .map({ .<title>.subst(/^'Category:'/, '') => .<categoryinfo><pages> || 0 });
 
my $out = open($tablefile, :w)  or die "$!\n";
 
# Add table boilerplate and header
$out.say:
    "\{|class=\"wikitable sortable\"\n",
    "|+ As of { Date.today } :: {+%counts} Languages\n",
    "! Rank !! Language !! Count"
;
 
my @bg = <#fff; #ccc;>;
my $ff = 0;
my $rank = 1;
my $ties = 0;
 
# Get sorted unique task counts
for %counts.values.unique.sort: -* -> $count {
    $ff++;
    # Get list of tasks with this count
    my @these = %counts.grep( *.value == $count )».keys.sort: *.&naturally;
 
    for @these {
        $ties++;
        $out.say:
          "|-\n"~
          "|style=\"background-color: { @bg[$ff % 2] }\"|$rank\n"~
          "|style=\"background-color: { @bg[$ff % 2] }\"|[[:Category:$_|]]\n"~
          "|style=\"background-color: { @bg[$ff % 2] }\"|$count";
    }
    $rank += $ties;
    $ties = 0;
}
$out.say( "|}\n" );
$out.close;
 
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


### Perl 6: Using web scraping



Scraping the languages and categories pages. Perl 6 automatically handles Unicode names correctly.

```raku
my $languages =  qx{wget -O - 'http://rosettacode.org/wiki/Category:Programming_Languages'};
my $categories = qx{wget -O - 'http://www.rosettacode.org/mw/index.php?title=Special:Categories&limit=5000'};
 
my @lines = $languages.lines;
shift @lines until @lines[0] ~~ / '<h2>Subcategories</h2>' /;
my \languages = set gather for @lines {
    last if / '/bodycontent' /;
    take ~$0 if
        / '<li><a href="/wiki/Category:' .*? '" title="Category:' .*? '">' (.*?) '</a></li>' /;
}
 
@lines = $categories.lines;
my @results = sort -*.[0], gather for @lines {
    take [+$1, ~$0] if
        / '<li><a href="/wiki/Category:' .*? '" title="Category:' .*? '">'
        (.*?) <?{ ~$0 ∈ languages }>
        '</a>' .*? '(' (\d+) ' member' /;
}
 
for @results.kv -> $i, @l {
    printf "%d:\t%3d - %s\n", $i+1, |@l;
}
```


(As of 2014-07-11.) Here we show only the top 10.


#### Output:
```
1:      833 - Tcl
2:      781 - Racket
3:      770 - Python
4:      730 - Perl 6
5:      725 - J
6:      712 - C
7:      708 - Ruby
8:      698 - D
9:      674 - Go
10:     656 - Perl
```