[1]: https://rosettacode.org/wiki/Rosetta_Code/Find_unimplemented_tasks

# [Rosetta Code/Find unimplemented tasks][1]



```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
use Sort::Naturally;

unit sub MAIN( Str :$lang = 'Raku' );

my $client = HTTP::UserAgent.new;
my $url = 'https://rosettacode.org/w';

my @total;
my @impl;

@total.append: .&get-cat for 'Programming_Tasks', 'Draft_Programming_Tasks';
@impl = get-cat $lang;

say "Unimplemented tasks in $lang:";
my @unimplemented = (@total (-) @impl).keys.sort: *.&naturally;
.say for @unimplemented;
say "{+@unimplemented} unimplemented tasks";

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
        :action<query>, :format<json>, :formatversion<2>, |%query);
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

#### Output:
```
Unimplemented tasks in Raku:
3d turtle graphics
15 puzzle game in 3D
Addition-chain exponentiation
Audio alarm
Audio frequency generator
Audio overlap loop
Binary coded decimal
Black box
Blackjack strategy
Boids
Catmull–Clark subdivision surface
Chess player
CLI-based maze-game
Compare sorting algorithms' performance
Compiler/AST interpreter
Compiler/Preprocessor
Compiler/syntax analyzer
Create an executable for a program in an interpreted language
Cross compilation
Cycles of a permutation
Diophantine linear system solving
Dominoes
Elevator simulation
Execute Computer/Zero
Free polyominoes enumeration
Generalised floating point multiplication
Hexapawn
Honeycombs
IRC gateway
Morpion solitaire
Number triplets game
OLE automation
OpenGL pixel shader
P-Adic square roots
Play recorded sounds
Red black tree sort
Remote agent/Agent interface
Remote agent/Agent logic
Remote agent/Simulation
Simple turtle graphics
Solve a Rubik's cube
Solving coin problems
Sorting algorithms/Tree sort on a linked list
Tamagotchi emulator
Tetris
Ukkonen’s suffix tree construction
Unicode polynomial equation
Uno (card game)
Use a REST API
Waveform analysis/Top and tail
Weather routing
WebGL rotating F
52 unimplemented tasks
```
