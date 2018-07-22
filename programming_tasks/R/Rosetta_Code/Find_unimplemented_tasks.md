[1]: https://rosettacode.org/wiki/Rosetta_Code/Find_unimplemented_tasks

# [Rosetta Code/Find unimplemented tasks][1]

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
use Sort::Naturally;
 
unit sub MAIN( Str :$lang = 'Perl_6' );
 
my $client = HTTP::UserAgent.new;
my $url = 'http://rosettacode.org/mw';
 
my @total;
my @impl;
 
@total.append: .&get-cat for 'Programming_Tasks', 'Draft_Programming_Tasks';
@impl = get-cat $lang;
 
say "Unimplemented tasks in $lang:";
.say for (@total (-) @impl).keys.sort: *.&naturally;
 
sub get-cat ($category) {
    flat mediawiki-query(
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle("Category:$category"),
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<title>
    ).map({ .<title> });
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
 
sub uri-query-string (*%fields) { %fields.map({ "{.key}={uri-escape .value}" }).join("&") }
```