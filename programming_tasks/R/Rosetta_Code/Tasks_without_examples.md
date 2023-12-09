[1]: https://rosettacode.org/wiki/Rosetta_Code/Tasks_without_examples

# [Rosetta Code/Tasks without examples][1]

```perl
use HTTP::UserAgent;
use Gumbo;
 
my $ua = HTTP::UserAgent.new;
my $taskfile = './RC_tasks.html';
 
# Get list of Tasks
say "Updating Programming_Tasks list...";
my $page   = "https://rosettacode.org/wiki/Category:Programming_Tasks";
my $html   = $ua.get($page).content;
my $xmldoc = parse-html($html, :TAG<div>, :id<mw-pages>);
my @tasks  = parse-html($xmldoc[0].Str, :TAG<li>).Str.comb( /'/wiki/' <-["]>+ / )».substr(6); #"
my $f      = open("./RC_Programming_Tasks.txt", :w)  or die "$!\n";
note "Writing Programming_Tasks file...";
$f.print( @tasks.join("\n") );
$f.close;
 
sleep .5;
 
for 'Programming_Tasks' -> $category
{ # Scrape info from each page.
 
    note "Loading $category file...";
    note "Retreiving tasks...";
    my @entries = "./RC_{$category}.txt".IO.slurp.lines;
 
    for @entries -> $title {
        note $title;
 
        # Get the raw page
        my $html = $ua.get: "https://rosettacode.org/wiki/{$title}";
 
        # Filter out the actual task description
        $html.content ~~ m|'<div id="mw-content-text" lang="en" dir="ltr" class="mw-content-ltr"><div'
                            .+? 'using any language you may know.</div>' (.+?) '<div id="toc"'|;
 
        my $task = cleanup $0.Str;
 
        # save to a file
        my $fh = $taskfile.IO.open :a;
 
        $fh.put: "<hr>\n     $title\n<hr>\n$task";
 
        $fh.close;
 
        sleep 3; # Don't pound the server
    }
}
 
sub cleanup ( $string ) {
    $string.subst( /^.+ '</div>'/, '' )
}
```

#### Output:
```
    100_doors
```


There are 100 doors in a row that are all initially closed.



You make 100 &lt;a href="/wiki/Rosetta\_Code:Multiple\_passes" title="Rosetta Code:Multiple passes"&gt;passes&lt;/a&gt; by the doors.



The first time through, visit every door and &#160;*toggle*&#160; the door &#160;(if the door is closed, &#160;open it; &#160; if it is open,&#160; close it).



The second time, only visit every 2<sup>nd</sup> door &#160; (door #2, #4, #6, ...), &#160; and toggle it.



The third time, visit every 3<sup>rd</sup> door &#160; (door #3, #6, #9, ...), etc, &#160; until you only visit the 100<sup>th</sup> door.








Answer the question: &#160; what state are the doors in after the last pass? &#160; Which are open, which are closed?





<p>**&lt;a href="/wiki/Rosetta\_Code:Extra\_credit" title="Rosetta Code:Extra credit"&gt;Alternate&lt;/a&gt;:**
As noted in this page's &#160; &lt;a href="/wiki/Talk:100\_doors" title="Talk:100 doors"&gt;discussion page&lt;/a&gt;, &#160; the only doors that remain open are those whose numbers are perfect squares.



Opening only those doors is an &#160; &lt;a href="/wiki/Rosetta\_Code:Optimization" title="Rosetta Code:Optimization"&gt;optimization&lt;/a&gt; &#160; that may also be expressed;
<p>however, as should be obvious, this defeats the intent of comparing implementations across programming languages.





#### Output:
```
    15_Puzzle_Game
```


Implement the &lt;a href="[https://en.wikipedia.org/wiki/15_puzzle](https://en.wikipedia.org/wiki/15_puzzle)" class="extiw" title="wp:15 puzzle"&gt;Fifteen Puzzle Game&lt;/a&gt;.











#### Output:
```
    15_puzzle_solver
```


Your task is to write a program that finds a solution in the fewest single moves (no multimoves) possible to a random &lt;a href="[https://en.wikipedia.org/wiki/15_puzzle](https://en.wikipedia.org/wiki/15_puzzle)" class="extiw" title="wp:15 puzzle"&gt;Fifteen Puzzle Game&lt;/a&gt;.

For this task you will be using the following puzzle:



#### Output:
```
15 14  1  6
 9 11  4 12
 0 10  7  3
13  8  5  2
```






#### Output:
```
 1  2  3  4
 5  6  7  8
 9 10 11 12
13 14 15  0
```


The output must show the moves' directions, like so: left, left, left, down, right... and so on.

There are 2 solutions with 52 moves:

rrrulddluuuldrurdddrullulurrrddldluurddlulurruldrdrd

rrruldluuldrurdddluulurrrdlddruldluurddlulurruldrrdd

finding either one, or both is an acceptable result.

see: &lt;a rel="nofollow" class="external text" href="[http://www.rosettacode.org/wiki/15_puzzle_solver/Optimal_solution](http://www.rosettacode.org/wiki/15_puzzle_solver/Optimal_solution)"&gt;Pretty Print of Optimal Solution&lt;/a&gt;



Solve the following problem:


#### Output:
```
  0 12  9 13
 15 11 10 14
  3  7  2  5
  4  8  6  1
```









**...and so on... **