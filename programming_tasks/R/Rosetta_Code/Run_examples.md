[1]: https://rosettacode.org/wiki/Rosetta_Code/Run_examples

# [Rosetta Code/Run examples][1]





This is a fairly comprehensive task code runner. It is set up to work for Raku by default, but has basic configurations to run Perl, Python, Tcl and Go tasks as well. It can be easily tweaked to work with other languages by adding a load-lang('whatever'){} routine similar to the Raku, Perl, Python, Tcl and Go ones. (And ensuring that the appropriate compiler is installed and accessible.) There is so much variation to the task requirements and calling conventions that it would be problematic to make a general purpose, language agnostic code runner so some configuration is necessary to make it work with other languages.



(Note that there is no dependency download code nor resource hash for Python and Go, though they can run a remarkable number of tasks without.)



By default, this will download the Raku section of any (every) task that has a Raku example, extract the code blocks and attempt to run them. Many tasks require files or user interaction to proceed, others are not complete runnable code blocks (example code fragments), some tasks run forever.  To try to deal with and compensate for this, this implementation can load a %resource hash that will: supply input files where necessary, skip unrunnable code fragments, limit long and/or infinite running blocks, supply user interaction code where possible, and skip blocks where user interaction is unavoidable.



There are several command line options to control its actions. See the README in the repository for details.



The complete implementation is too large and cumbersome to post in it's entirety here, only the main task retrieval and execution code is included.



For the whole ball of wax see [the rc-run github repository](https://github.com/thundergnat/rc-run).



Run with no parameters to run every implemented task on Rosetta Code. Feed it a task name to only download / run that task. Give it command line switches to adjust its behaviour.



Note: This is set up to run under Linux. It could be adapted for Windows (or OSX I suppose) fairly easily but I don't have access to those OSs, nor do I care to seek it.

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
use Text::Levenshtein::Damerau;
use MONKEY-SEE-NO-EVAL;

#####################################
say "Version = 2020-03-15T12:15:31";
#####################################

sleep 1;

my %*SUB-MAIN-OPTS = :named-anywhere;

unit sub MAIN(
    Str $run = '',        #= Task or file name
    Str :$lang = 'raku',  #= Language, default raku - used to load configuration settings
    Int :$skip = 0,       #= Skip # to continue partially into a list
    Bool :f(:$force),     #= Override any task skip parameter in %resource hash
    Bool :l(:$local),     #= Only use code from local cache
    Bool :r(:$remote),    #= Only use code from remote server (refresh local cache)
    Bool :q(:$quiet),     #= Less verbose, don't display source code
    Bool :d(:$deps),      #= Load dependencies
    Bool :p(:$pause),     #= pause after each task
    Bool :b(:$broken),    #= pause after each task which is broken or fails in some way
    Int  :$sleep = 0,     #= sleep for $sleep after each task
    Bool :t(:$timer),     #= save timing data for each task
);

die 'You can select local or remote, but not both...' if $local && $remote;

## INITIALIZATION

my $client   = HTTP::UserAgent.new;
my $url      = 'https://rosettacode.org/w';

my %c = ( # text colors
    code  => "\e[0;92m", # green
    delim => "\e[0;93m", # yellow
    cmd   => "\e[1;96m", # cyan
    bad   => "\e[0;91m", # red
    warn  => "\e[38;2;255;155;0m", # orange
    dep   => "\e[38;2;248;24;148m", # pink
    clr   => "\e[0m",    # clear formatting
);

my $view      = 'xdg-open';       # image viewer, this will open default under Linux
my %l         = load-lang($lang); # load language parameters
my %resource  = load-resources($lang);
my $get-tasks = True;

my @tasks;

run('clear');

## FIGURE OUT WHICH TASKS TO RUN

if $run {
    if $run.IO.e and $run.IO.f {# is it a file?
        @tasks = $run.IO.lines; # yep, treat each line as a task name
    } else {                    # must be a single task name
        @tasks = ($run);        # treat it so
    }
    $get-tasks = False;         # don't need to retrieve task names from web
}

if $get-tasks { # load tasks from web if cache is not found, older than one day or forced
    if !"%l<dir>.tasks".IO.e or (now - "%l<dir>.tasks".IO.modified) > 86400 or $remote {
        note 'Retrieving task list from site.';
        @tasks = mediawiki-query( # get tasks from web
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle("Category:%l<language>"),
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<title>
        )»<title>.grep( * !~~ /^'Category:'/ ).sort;
        "%l<dir>.tasks".IO.spurt: @tasks.sort.join("\n");
    } else {
        note 'Using cached task list.';
        @tasks = "%l<dir>.tasks".IO.slurp.lines; # load tasks from file
    }
}

my $tfile;
if $timer {
    $tfile = open :w, "{$lang}-time.txt";
    $tfile.close;
}

note "Skipping first $skip tasks..." if $skip;
my $redo;

## MAIN LOOP

for @tasks -> $title {
    $redo = False;
    next if $++ < $skip;
    next unless $title ~~ /\S/; # filter blank lines (from files)
    say my $tasknum = $skip + ++$, ")  $title";

    my $name = $title.subst(/<-[-0..9A..Za..z]>/, '_', :g);
    my $taskdir = "./rc/%l<dir>/$name";

    my $modified = "$taskdir/$name.txt".IO.e ?? "$taskdir/$name.txt".IO.modified !! 0;

    my $entry;
    if $remote or !"$taskdir/$name.txt".IO.e or ((now - $modified) > 86400 * 7) {
        my $page = $client.get("{ $url }/index.php?title={ uri-escape $title }&action=raw").content;

        uh-oh("Whoops, can't find page: $url/$title :check spelling.\n\n{fuzzy-search($title)}", 'warn')
            and next if $page.elems == 0;
        say "Getting code from: https://rosettacode.org/wiki/{ $title.subst(' ', '_', :g) }#%l<language>";

        $entry = $page.comb(rx:i/'=={{header|' $(%l<header>) '}}==' .+? [<?before \n'=='<-[={]>*'{{header'> || $] /).Str //
          uh-oh("No code found\nMay be bad markup", 'warn');

        if $entry ~~ /^^ 'See [[' (.+?) '/' $(%l<language>) / { # no code on main page, check sub page
            $entry = $client.get("{ $url }/index.php?title={ uri-escape $/[0].Str ~ '/' ~ %l<language> }&action=raw").content;
        }
        mkdir $taskdir unless $taskdir.IO.d;
        spurt( "$taskdir/$name.txt", $entry );
    } else {
        if "$taskdir/$name.txt".IO.e {
            $entry = "$taskdir/$name.txt".IO.slurp;
            say "Loading code from: $taskdir/$name.txt";
        } else {
            uh-oh("Task code $taskdir/$name.txt not found, check spelling or run remote.", 'warn');
            next;
        }
    }

    my @blocks = $entry.comb: %l<tag>;

    unless @blocks {
        uh-oh("No code found\nMay be bad markup", 'warn') unless %resource{"$name"}<skip> ~~ /'ok to skip'/;
        say "Skipping $name: ", %resource{"$name"}<skip>, "\n" if %resource{"$name"}<skip>
    }

    for @blocks.kv -> $k, $v {
        my $n = +@blocks == 1 ?? '' !! $k;
        spurt( "$taskdir/$name$n%l<ext>", $v );
        if %resource{"$name$n"}<skip> && !$force {
            dump-code ("$taskdir/$name$n%l<ext>");
            if %resource{"$name$n"}<skip> ~~ /'broken'/ {
                uh-oh(%resource{"$name$n"}<skip>, 'bad');
                pause if $broken;
            } else {
                say "{%c<warn>}Skipping $name$n: ", %resource{"$name$n"}<skip>, "{%c<clr>}\n";
            }
            next;
        }
        say "\nTesting $name$n";
        run-it($taskdir, "$name$n", $tasknum);
    }
    say  %c<delim>, '=' x 79, %c<clr>;
    redo if $redo;
    sleep $sleep if $sleep;
    pause if $pause;
}

## SUBROUTINES

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

sub run-it ($dir, $code, $tasknum) {
    my $current = $*CWD;
    chdir $dir;
    if %resource{$code}<file> -> $fn {
        copy "$current/rc/resources/{$_}", "./{$_}" for $fn[]
    }
    dump-code ("$code%l<ext>") unless $quiet;
    check-dependencies("$code%l<ext>", $lang) if $deps;
    my @cmd = %resource{$code}<cmd> ?? |%resource{$code}<cmd> !! "%l<exe> $code%l<ext>\n";
    if $timer {
        $tfile = open :a, "{$current}/{$lang}-time.txt";
    }
    my $time = 'NA: not run or killed before completion';
    for @cmd -> $cmd {
        say "\nCommand line: {%c<cmd>}$cmd",%c<clr>;
        if $timer { $tfile.say: "Command line: $cmd".chomp }
        my $start = now;
        try shell $cmd;
        $time = (now - $start).round(.001);
        CATCH {
            when /'exit code: 137'/ { }
            default {
                .resume unless $broken;
                uh-oh($_, 'bad');
                if %resource{$code}<fail-by-design> {
                    say %c<warn>, 'Fails by design, (or at least, it\'s not unexpected).', %c<clr>;
                } else {
                    if pause.lc eq 'r' {
                       unlink "$code.txt";
                       $redo = True;
                    }
                }
             }
        }
    if $timer { $tfile.say("#$tasknum - Wallclock seconds: $time\n") }
    }
    chdir $current;
    say "\nDone task #$tasknum: $code - wallclock seconds: $time\e[?25h";
    $tfile.close if $timer;
}

sub pause {
    prompt "Press enter to procede:> ";
    # or
    # sleep 5;
}

sub dump-code ($fn) {
    say "\n", %c<delim>, ('vvvvvvvv' xx 7).join(' CODE '), %c<clr>, "\n", %c<code>;
    print $fn.IO.slurp;
    say %c<clr>,"\n\n",%c<delim>,('^^^^^^^^' xx 7).join(' CODE '),%c<clr>;
}

sub uri-query-string (*%fields) { %fields.map({ "{.key}={uri-escape .value}" }).join('&') }

sub clear { "\r" ~ ' ' x 100 ~ "\r" }

sub uh-oh ($err, $class='warn') { put %c{$class}, "{'#' x 79}\n\n $err \n\n{'#' x 79}", %c<clr> }

sub fuzzy-search ($title) {
    my @tasknames;
    if "%l<dir>.tasks".IO.e {
        @tasknames = "%l<dir>.tasks".IO.slurp.lines;
    }
    return '' unless @tasknames.elems;
    " Did you perhaps mean:\n\n\t" ~
    @tasknames.grep( {.lc.contains($title.lc) or dld($_, $title) < (5 min $title.chars)} ).join("\n\t");
}                # Damerau Levenshtein distance  ^^^

multi check-dependencies ($fn, 'raku') {
    my @use = $fn.IO.slurp.comb(/<?after ^^ \h* 'use '> \N+? <?before \h* ';'>/);
    if +@use {
        say %c<dep>, 'Checking dependencies...', %c<clr>;
        for @use -> $module {
            if $module eq any('v6', 'v6.c', 'v6.d', 'nqp', 'NativeCall', 'Test') or $module.contains('MONKEY')
              or $module.contains('experimental') or $module.starts-with('lib') or $module.contains('from<Perl5>') {
                print %c<dep>;
                say 'ok, no installation necessary: ', $module;
                print %c<clr>;
                next;
            }
            my $installed = $*REPO.resolve(CompUnit::DependencySpecification.new(:short-name($module)));
            my @mods = $module;
            if './../../../raku-modules.txt'.IO.e {
                my $fh = open( './../../../perl6-modules.txt', :r ) or die $fh;
                @mods.append: $fh.lines;
                $fh.close;
            }
            my $fh = open( './../../../raku-modules.txt', :w ) or die $fh;
            $fh.spurt: @mods.Bag.keys.sort.join: "\n";
            $fh.close;
            print %c<dep>;
            if $installed {
                say 'ok, installed: ', $module
            } else {
                say 'not installed: ', $module;
                shell("zef install $module");
            }
            print %c<clr>;
        }
    }
}

multi check-dependencies ($fn, 'perl') {
    my @use = $fn.IO.slurp.comb(/<?after ^^ \h* 'use '> \N+? <?before \h* ';'>/);
    if +@use {
        for @use -> $module {
            next if $module eq $module.lc;
            next if $module.starts-with(any('constant','bignum'));
            my $installed = shell( "%l<exe> -e 'eval \"use {$module}\"; exit 1 if \$@'" );
            print %c<dep>;
            if $installed {
                say 'ok:            ', $module
            } else {
                say 'not installed: ', $module;
                try shell("sudo cpan $module");
            }
            print %c<clr>;
        }
    }
}

multi check-dependencies  ($fn, $unknown) {
    note "Sorry, don't know how to handle dependencies for $unknown language."
};

multi load-lang ('raku') { ( # Language specific variables. Adjust to suit.
    language => 'Raku',  # language category name
    exe      => 'raku',  # executable name to run perl6 in a shell
    ext      => '.raku', # file extension for perl6 code (optional, but nice to have)
    dir      => 'raku',  # directory to save tasks to
    header   => 'Raku',  # header text (=={{header|Raku}}==)
    # tags marking blocks of code - spaced out to placate wiki formatter
    # and to avoid getting tripped up when trying to run _this_ task.
    # note that this tag only selects the syntax highlighting, continue to
    # leave 'perl6' as an option, 'raku' is now live on the site.
    tag => rx/:i <?after '<syntaxhighlight lang="' ['perl6'|'raku'] '"' ' line'? '>' > .*? <?before '</' 'syntaxhighlight>'>/,
) }

multi load-lang ('perl') { (
    language => 'Perl',
    exe      => 'perl',
    ext      => '.pl',
    dir      => 'perl',
    header   => 'Perl',
    tag => rx/:i <?after '<syntaxhighlight lang="' 'perl' '"' ' line'? '>' > .*? <?before '</' 'lang>'>/,
) }

multi load-lang ('python') { (
    language => 'Python',
    exe      => 'python',
    ext      => '.py',
    dir      => 'python',
    header   => 'Python',
    tag => rx/:i <?after '<syntaxhighlight lang="' 'python' '"' ' line'? '>' > .*? <?before '</' 'lang>'>/,
) }

multi load-lang ('go') { (
    language => 'Go',
    exe      => 'go run',
    ext      => '.go',
    dir      => 'go',
    header   => 'Go',
    tag => rx/:i <?after '<syntaxhighlight lang="' 'go'  '"' ' line'? '>' > .*? <?before '</' 'lang>'>/,
) }

multi load-lang ('tcl') { (
    language => 'Tcl',
    exe      => 'tclsh',
    ext      => '.tcl',
    dir      => 'tcl',
    header   => 'Tcl',
    tag => rx/:i <?after '<syntaxhighlight lang="' 'tcl'  '"' ' line'? '>' > .*? <?before '</' 'lang>'>/,
) }

multi load-lang ($unknown) { die "Sorry, don't know how to handle $unknown language." };

multi load-resources ($unknown) { () };
```

#### Output:
```
Retrieving tasks
1)  Determine if a string is numeric
Getting code from: https://rosettacode.org/wiki/Determine_if_a_string_is_numeric#Raku

Testing Determine_if_a_string_is_numeric

Command line: raku Determine_if_a_string_is_numeric.p6

               Coerce     Don't coerce
    String   whitespace    whitespace
       <1>      True         True
     <1.2>      True         True
   <1.2.3>     False        False
      <-6>      True         True
     <1/2>      True         True
     <12e>     False        False
     <B17>     False        False
 <1.3e+12>      True         True
  <1.3e12>      True         True
 <-2.6e-3>      True         True
    <zero>     False        False
      <0x>     False        False
   <0xA10>      True         True
  <0b1001>      True         True
    <0o16>      True         True
    <0o18>     False        False
    <2+5i>      True         True
    <True>     False        False
   <False>     False        False
     <Inf>      True         True
     <NaN>      True         True
 <0x10.50>      True         True
   <0b102>     False        False
  <0o_5_3>      True         True
      <௫௯>      True         True
  <  12  >      True         True
   <1 1 1>     False        False
        <>      True        False
       < >      True        False

Done task #1: Determine_if_a_string_is_numeric - wallclock seconds: 0.171
===============================================================================
```


Or, if the full %resource hash is loaded it will automatically feed input parameters to tasks that require them:

**raku RC-run.p6 Lucky\_and\_even\_lucky\_numbers** -q


```
Retrieving tasks
1 Lucky_and_even_lucky_numbers
Getting code from: https://rosettacode.org/wiki/Lucky_and_even_lucky_numbers#Raku

Testing Lucky_and_even_lucky_numbers

Command line: raku Lucky_and_even_lucky_numbers.p6 20 , lucky

79

Command line: raku Lucky_and_even_lucky_numbers.p6 1 20

(1 3 7 9 13 15 21 25 31 33 37 43 49 51 63 67 69 73 75 79)

Command line: raku Lucky_and_even_lucky_numbers.p6 1 20 evenlucky

(2 4 6 10 12 18 20 22 26 34 36 42 44 50 52 54 58 68 70 76)

Command line: raku Lucky_and_even_lucky_numbers.p6 6000 -6100

(6009 6019 6031 6049 6055 6061 6079 6093)

Command line: raku Lucky_and_even_lucky_numbers.p6 6000 -6100 evenlucky

(6018 6020 6022 6026 6036 6038 6050 6058 6074 6090 6092)

Done Lucky_and_even_lucky_numbers
===============================================================================
```


Try running a Go task - Command line: **raku RC-run.p6 -q --lang=go "Determine if a string is numeric"**



Finds two task entries, downloads both and runs each:


```
1)  Determine if a string is numeric
Getting code from: https://rosettacode.org/wiki/Determine_if_a_string_is_numeric#Go

Testing Determine_if_a_string_is_numeric0

Command line: go run Determine_if_a_string_is_numeric0.go

Are these strings numeric?
     1 -> true
  3.14 -> true
  -100 -> true
   1e2 -> true
   NaN -> true
  rose -> false

Done task #1: Determine_if_a_string_is_numeric0 - wallclock seconds: 0.16

Testing Determine_if_a_string_is_numeric1

Command line: go run Determine_if_a_string_is_numeric1.go

Are these strings integers?
    1 -> true
  one -> false

Done task #1: Determine_if_a_string_is_numeric1 - wallclock seconds: 0.147
===============================================================================
```
