[1]: https://rosettacode.org/wiki/Rosetta_Code/Rank_languages_by_popularity

# [Rosetta Code/Rank languages by popularity][1]





### Raku: Using the API



Note that this counts **only** the tasks. It does not include other non-task categories in the counts yielding more realistic, non-inflated numbers. Raku is Unicode aware and handles non-ASCII names natively. This does not attempt to 'unify' different language names that are the same behind the scenes as a result of Rosettacodes' capitalization peculiarities. (E.G. μC++, UC++ &amp; ΜC++)

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
use Sort::Naturally;

my $client = HTTP::UserAgent.new;

my $url = 'https://rosettacode.org/w';

my $tablefile = './RC_Popularity.txt';

my %cat = (
    'Programming_Tasks' => 'Task',
    'Draft_Programming_Tasks' => 'Draft'
);
my %tasks;

for %cat.keys.sort -> $cat {
    mediawiki-query(
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle("Category:$cat"),
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<title>
    ).map({ %tasks{%cat{$cat}}++ });
}

my %counts =
    mediawiki-query(
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle<Category:Programming Languages>,
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<categoryinfo>
    )
    .map({
        my $title = .<title>.subst(/^'Category:'/, '');
        my $tasks = (.<categoryinfo><pages> || 0);
        my $categories = (.<categoryinfo><subcats> || 0);
        my $total = (.<categoryinfo><size> || 0);
       $title  => [$tasks ,$categories, $total]
   });

my $out = open($tablefile, :w)  or die "$!\n";

# Add table boilerplate and header
$out.say:
    "\{|class=\"wikitable sortable\"\n",
    "|+ As of { Date.today } :: {+%counts} Languages\n",
    '!Rank!!Language!!Task<br>Entries!!Tasks<br>done %!!Non-task<br>Subcate-<br>gories!!Total<br>Categories'
;

my @bg = <#fff; #ccc;>;
my $ff = 0;
my $rank = 1;
my $ties = 0;

# Get sorted unique task counts
for %counts.values»[0].unique.sort: -* -> $count {
    $ff++;
    # Get list of tasks with this count
    my @these = %counts.grep( *.value[0] == $count )».keys.sort: *.&naturally;

    for @these {
        $ties++;
        $out.say:
          "|- style=\"background-color: { @bg[$ff % 2] }\"\n"~
          "|$rank\n"~
          "|[[:Category:$_|]]\n"~
          "|$count\n"~
          "|{(100 * $count/%tasks<Draft Task>.sum).round(.01)} %\n"~
          "|{%counts{$_}[1]}\n"~
          "|{%counts{$_}[2]}"
    }
    $rank += $ties;
    $ties = 0;
}
$out.say( "|}" );
$out.say('=' x 5, " query, download & processing: {(now - INIT now).round(.01)} seconds ", '=' x 5);
$out.close;

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


### Raku: Using web scraping



Scraping the languages and categories pages.  Raku automatically handles Unicode names correctly.

```perl
my $languages =  qx{wget -O - 'https://rosettacode.org/wiki/Category:Programming_Languages'};
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
    take [+$1.subst(',', ''), ~$0] if
        / '<li><a href="/wiki/Category:' .*? '" title="Category:' .*? '">'
        (.*?) <?{ ~$0 ∈ languages }>
        '</a>' .*? '(' (<[, 0..9]>+) ' member' /;
}
 
for @results.kv -> $i, @l {
    printf "%d:\t%4d - %s\n", $i+1, |@l;
}
```


(As of 2018-10-28) Here we show only the top 10.


```
1:      1036 - Go
2:      1029 - Kotlin
3:      1014 - Python
4:       998 - Perl 6
5:       980 - Racket
6:       929 - Tcl
7:       926 - C
8:       910 - Perl
9:       904 - Zkl
10:      901 - J
```
