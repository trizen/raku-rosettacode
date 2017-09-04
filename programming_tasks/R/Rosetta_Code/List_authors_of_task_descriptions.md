[1]: http://rosettacode.org/wiki/Rosetta_Code/List_authors_of_task_descriptions

# [Rosetta Code/List authors of task descriptions][1]

The pseudocode above is no longer really useful as the page format has changed. Rather than checking **every** edit to see if it was a change to the task description, we'll just assume the user that created the page is the task author. This isn't 100% accurate; a very few pages got renamed and recreated by someone other than the original author without preserving the history, so they are misreported (15 Puzzle Game for instance,) but is as good as it is likely to get without extensive manual intervention. Any further edits to the task description are not credited. As it is, we must still make *thousands* of requests and pound the server pretty hard. Checking **every** edit would make the task several of orders of magnitude more abusive of the server (and my internet connection.)



Each stage of the scraping process is saved to local files so it can be restarted without losing all your progress in the event of a timeout or error. If that happens though, you need to manually adjust where to restart the process.

```perl
use HTTP::UserAgent;
use Gumbo;
use Sort::Naturally;
 
my $ua = HTTP::UserAgent.new;
 
for 'Programming_Tasks', 'Draft_Programming_Tasks' -> $category
{ # Get lists of Tasks & Draft Tasks
    # last; # Uncomment to skip this step
    my $page = "http://rosettacode.org/wiki/Category:$category";
    my $html =  $ua.get($page).content;
    my $xmldoc = parse-html($html, :TAG<div>, :id<mw-pages>);
    my @tasks = parse-html($xmldoc[0].Str, :TAG<li>).Str.comb( /'/wiki/' <-["]>+ / )>>.substr(6); #"
    my $f = open("./RC_{$category}.txt", :w)  or die "$!\n";
    $f.print( @tasks.join("\n") );
    $f.close;
}
 
for 'Programming_Tasks', 'Draft_Programming_Tasks' -> $category
{ # Scrape info from each page.
    # last; # Uncomment to skip this step
    my @tasks = "./RC_{$category}.txt".IO.slurp.lines;
 
    for @tasks -> $title {
 
        my $ua = HTTP::UserAgent.new;
        # Get the earliest edit
        my $addr = "http://rosettacode.org/mw/index.php?title={$title}&dir=prev&limit=1&action=history";
 
        my $html = $ua.get: $addr;
 
        $html.content ~~ m|'<li><span class="mw-history-histlinks">' (.+?) '</ul>' |;
 
        my $line = $0.lines.tail;
        # Parse out the User name
        $line ~~ m| 'title="User:' <-[>]>+? '>' (.+?) '</a>' |;
 
        my $auth = $0;
        # Oops, no user name, must be anonymous, get IP address instead
        unless $auth {
            $line ~~ m| '"mw-userlink mw-anonuserlink">' (.+?) '</a>' |;
            $auth = $0;
        }
 
        # Parse out human readable title
        $line ~~ m| '<a href="/mw/index.php?title=' $title '&amp;' .+? 'title="'(.+?)'">cur</a>' |;
 
        my $decoded = $0;
 
        # report progress
        say "$decoded: $auth";
 
        # save it to a file
        my $f = open("./RC_Authors.txt", :a)  or die "$!\n";
        $f.say( "[[$title|$decoded]]\t$category\t$auth" );
        $f.close;
 
        sleep 3; # Don't pound the server
    }
    sleep 300; # Wait between batches, seems to disconnect after 1000 requests without pause
}
 
my %authors;
 
# Generate an HTML table from the results.
my ($cnt, $draftcnt, $taskcnt);
"./RC_Authors.txt".IO.slurp.lines.map: {
    my ($task, $cat, $auth) = $_.split("\t");
    $cnt++;
    if $cat.contains('Draft') {
        $cat = 'Draft:';
        $draftcnt++;
    } else {
        $cat = 'Task: ';
        $taskcnt++;
    }
    %authors{$auth}.push: "$cat $task";
};
 
# Dump an HTML table to STDOUT, capture it for display
say '<table border="1"><tr><th colspan="2">As of ', Date.today, ' | Total: ',
    $cnt, ' / Tasks: ', $taskcnt, ' / Draft Tasks: ', $draftcnt,
    '<tr><th>User</th><th>Authored</th></tr>';
for %authors.sort(*.key.&naturally) -> $a {
    print '<tr><td>', $a.key, '</td><td><ol><li>';
    print $a.value.sort( *.substr(7) ).join('</li><li>');
    say '</ol></td></tr>';
}
say '</table>';
```