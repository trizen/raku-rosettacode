[1]: https://rosettacode.org/wiki/Rosetta_Code/Count_examples

# [Rosetta Code/Count examples][1]

Retrieves counts for both Tasks and Draft Tasks. Save / Display results as a sortable wikitable rather than a static list. Click on a column header to sort on that column. To do a secondary sort, hold down the shift key and click on a second column header. Tasks have a gray (default) background, Draft Tasks have a yellow background.



For a full output, see [Rosetta Code/Count examples/Full list](https://rosettacode.org/wiki/Rosetta_Code/Count_examples/Full_list)

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
 
# Friendlier descriptions for task categories
my %cat = (
    'Programming_Tasks' => 'Task',
    'Draft_Programming_Tasks' => 'Draft'
);
 
my $client = HTTP::UserAgent.new;
 
my $url = 'http://rosettacode.org/mw';
 
my $hashfile  = './RC_Task_count.json';
my $tablefile = './RC_Task_count.txt';
 
my %tasks;
 
# clear screen
run($*DISTRO.is-win ?? 'cls' !! 'clear');
 
#=begin update
 
note 'Retrieving task information...';
 
for %cat.keys -> $cat {
    mediawiki-query(
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle("Category:$cat"),
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<title>
    ).map({
        my $page =
          $client.get("{ $url }/index.php?title={ uri-escape .<title> }&action=raw").content;
        my $count = +$page.lc.comb(/ ^^'==' <-[\n=]>* '{{header|' \w+ \N+ '==' \h* $$ /);
        %tasks{.<title>} = {'cat' => %cat{$cat}, :$count};
        print clear, 1 + $++, ' ', %cat{$cat}, ' ', .<title>;
    })
}
 
print clear;
 
note "\nTask information saved to local file: {$hashfile.IO.absolute}";
$hashfile.IO.spurt(%tasks.&to-json);
 
#=end update
 
# Load information from local file
%tasks = $hashfile.IO.e ?? $hashfile.IO.slurp.&from-json !! ( );
 
# Convert saved task / author info to a table
note "\nBuilding table...";
my $count    = +%tasks;
my $taskcnt  = +%tasks.grep: *.value.<cat> eq %cat<Programming_Tasks>;
my $draftcnt = $count - $taskcnt;
my $total    = sum %tasks{*}»<count>;
 
# Dump table to a file
my $out = open($tablefile, :w)  or die "$!\n";
 
# Add table boilerplate and caption
$out.say:
    '{|class="wikitable sortable"', "\n",
    "|+ As of { Date.today } :: Tasks: { $taskcnt } ::<span style=\"background-color:#ffd\"> Draft Tasks:",
    "{ $draftcnt } </span>:: Total Tasks: { $count } :: Total Examples: { $total }\n",
    "! Count !! Task !! Category"
;
 
# Sort tasks by count then add row
for %tasks.sort: { [-.value<count>, .key] } -> $task {
    $out.say:
      ( $task.value<cat> eq 'Draft'
        ?? "|- style=\"background-color: #ffc\"\n"
        !! "|-\n"
      ),
      "| { $task.value<count> }\n",
      ( $task.key ~~ /\d/
        ?? "|data-sort-value=\"{ $task.key.&naturally }\"| [[{uri-escape $task.key}|{$task.key}]]\n"
        !! "| [[{uri-escape $task.key}|{$task.key}]]\n"
      ),
      "| { $task.value<cat> }"
}
 
$out.say( "|}" );
$out.close;
 
note "Table file saved as: {$tablefile.IO.absolute}";
 
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
 
sub naturally ($a) { $a.lc.subst(/(\d+)/, ->$/ {0~(65+$0.chars).chr~$0},:g) }
 
sub clear { "\r" ~ ' ' x 100 ~ "\r" }
```