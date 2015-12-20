[1]: http://rosettacode.org/wiki/Rosetta_Code/Rank_languages_by_popularity

# [Rosetta Code/Rank languages by popularity][1]

```perl6
use HTTP::UserAgent;
use JSON::Tiny;
 
.say for
    mediawiki-query('http://rosettacode.org/mw', 'pages',
                    generator => 'categorymembers',
                    gcmtitle  => "Category:Programming Languages",
                    prop      => 'categoryinfo')\
 
    .map({ .<categoryinfo><pages> || 0,
           .<title>.subst(/^'Category:'/, '') })\
 
    .sort(-*[0])\
 
    .map({ sprintf "%3d. %3d - %s", ++$, @$_ });
 
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


Scraping the languages and categories pages.  Perl 6 automatically handles Unicode names correctly.

```perl6
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