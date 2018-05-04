[1]: http://rosettacode.org/wiki/Rosetta_Code/Run_examples

# [Rosetta Code/Run examples][1]

This is a fairly comprehensive task code runner. It is set up to work for Perl 6 by default, but has basic configurations to run Perl and Python tasks as well. It can be easily tweaked to work with other languages by adding a load-lang('whatever'){} routine similar to the Perl6, Perl and Python ones. (And ensuring that the appropriate compiler is installed and accessible.) There is so much variation to the task requirements and calling conventions that it would be problematic to make a general purpose, language agnostic code runner so some configuration is necessary to make it work with other languages.



By default, this will download the Perl 6 section of any (every) task that has a Perl 6 example, extract the code blocks and attempt to run them. Many tasks require files or user interaction to proceed, others are not complete runnable code blocks (example code fragments), some tasks run forever. To try to deal with and compensate for this, this implementation can load a&#160;%resource hash that will: supply input files where necessary, skip unrunnable code fragments, limit long and/or infinite running blocks, supply user interaction code where possible, and skip blocks where user interaction is unavoidable.



There are several command line options to control its actions. See the README in the repository for details.



The complete implementation is too large and cumbersome to post in it's entirety here, only the main task retrieval and execution code is included.



For the whole ball of wax see [the rc-run github repository](https://github.com/thundergnat/rc-run).



Run with no parameters to run every implemented task on Rosetta Code. Feed it a task name to only download / run that task. Give it command line switches to adjust its behaviour.



Note: This is set up to run under Linux. It could be adapted for Windows (or OSX I suppose) fairly easily but I don't have access to those OSs, nor do I care to seek it.

```perl
use HTTP::UserAgent;
use URI::Escape;
use JSON::Fast;
use MONKEY-SEE-NO-EVAL;
 
my %*SUB-MAIN-OPTS = :named-anywhere;
 
unit sub MAIN(
    Str $run = '',        # Task or file name
    Str :$lang = 'perl6', # Language, default perl6 - should be same as in <lang *> markup
    Int :$skip = 0,       # Skip # to continue partially into a list
    Bool :f(:$force),     # Override any task skip parameter in %resource hash
    Bool :l(:$local),     # Only use code from local cache
    Bool :r(:$remote),    # Only use code from remote server (refresh local cache)
    Bool :q(:$quiet),     # Less verbose, don't display source code
    Bool :d(:$deps),      # Load dependencies
    Bool :p(:$pause),     # pause after each task
    Bool :b(:$broken),     # pause after each task marked broken
);
 
die 'You can select local or remote, but not both...' if $local && $remote;
 
my $client   = HTTP::UserAgent.new;
my $url      = 'http://rosettacode.org/mw';
 
my %c = ( # text colors
    code  => "\e[0;92m", # green
    delim => "\e[0;93m", # yellow
    cmd   => "\e[1;96m", # cyan
    warn  => "\e[0;91m", # red
    clr   => "\e[0m",    # clear formatting
);
 
my $view      = 'xdg-open';       # image viewer, this will open default under Linux
my %l         = load-lang($lang); # load languge parameters
my %resource  = load-resources($lang);
my $get-tasks = True;
 
my @tasks;
 
run('clear');
 
 
if $run {
    if $run.IO.e and $run.IO.f {# is it a file?
        @tasks = $run.IO.lines; # yep, treat each line as a task name
    } else {                    # must be a single task name
        @tasks = ($run);        # treat it so
    }
    $get-tasks = False;         # don't need to retrieve task names from web
}
 
if $get-tasks { # load tasks from web if cache is not found, older than one day or forced
    if !"%l<dir>.tasks".IO.e or ("%l<dir>.tasks".IO.modified - now) > 86400 or $remote {
        note 'Retrieving task list from site.';
        @tasks = mediawiki-query( # get tasks from web
        $url, 'pages',
        :generator<categorymembers>,
        :gcmtitle("Category:%l<language>"),
        :gcmlimit<350>,
        :rawcontinue(),
        :prop<title>
        )»<title>.grep( * !~~ /^'Category:'/ ).sort;
        "%l<dir>.tasks".IO.spurt: @tasks.sort.join("\n");
    } else {
        note 'Using cached task list.';
        @tasks = "%l<dir>.tasks".IO.slurp.lines; # load tasks from file
    }
}
 
note "Skipping first $skip tasks..." if $skip;
 
for @tasks -> $title {
    next if $++ < $skip;
    next unless $title ~~ /\S/; # filter blank lines (from files)
    say $skip + ++$, ")  $title";
 
    my $name = $title.subst(/<-[-0..9A..Za..z]>/, '_', :g);
    my $taskdir = "./rc/%l<dir>/$name";
 
    my $modified = "$taskdir/$name.txt".IO.e ?? "$taskdir/$name.txt".IO.modified !! 0;
 
    my $entry;
    if $remote or !"$taskdir/$name.txt".IO.e or (($modified - now) > 86400 * 7) {
        my $page = $client.get("{ $url }/index.php?title={ uri-escape $title }&action=raw").content;
 
        uh-oh("Whoops, can't find page: $url/$title :check spelling.") and next if $page.elems == 0;
        say "Getting code from: http://rosettacode.org/wiki/{ $title.subst(' ', '_', :g) }#%l<language>";
 
        my $header = %l<header>; # can't interpolate hash into regex
        $entry = $page.comb(/'=={{header|' $header '}}==' .+? [<?before \n'=='<-[={]>*'{{header'> || $] /).Str //
          uh-oh("No code found\nMay be bad markup");
 
        my $lang = %l<language>; # can't interpolate hash into regex
        if $entry ~~ /^^ 'See [[' (.+?) '/' $lang / { # no code on main page, check sub page
            $entry = $client.get("{ $url }/index.php?title={ uri-escape $/[0].Str ~ '/' ~ %l<language> }&action=raw").content;
        }
        mkdir $taskdir unless $taskdir.IO.d;
        spurt( "$taskdir/$name.txt", $entry );
    } else {
        if "$taskdir/$name.txt".IO.e {
            $entry = "$taskdir/$name.txt".IO.slurp;
            say "Loading code from: $taskdir/$name.txt";
        } else {
            uh-oh("Task code $taskdir/$name.txt not found, check spelling or run remote.");
            next;
        }
    }
 
    my @blocks = $entry.comb: %l<tag>;
 
    unless @blocks {
        uh-oh("No code found\nMay be bad markup") unless %resource{"$name"}<skip> ~~ /'ok to skip'/;
        say "Skipping $name: ", %resource{"$name"}<skip>, "\n" if %resource{"$name"}<skip>
    }
 
    for @blocks.kv -> $k, $v {
        my $n = +@blocks == 1 ?? '' !! $k;
        spurt( "$taskdir/$name$n%l<ext>", $v );
        if %resource{"$name$n"}<skip> && !$force {
            dump-code ("$taskdir/$name$n%l<ext>");
            if %resource{"$name$n"}<skip> ~~ /'broken'/ {
                uh-oh(%resource{"$name$n"}<skip>);
                pause if $broken;
            } else {
                say "Skipping $name$n: ", %resource{"$name$n"}<skip>, "\n";
            }
            next;
        }
        say "\nTesting $name$n";
        run-it($taskdir, "$name$n");
    }
    say  %c<delim>, '=' x 79, %c<clr>;
    pause if $pause;
 
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
 
sub run-it ($dir, $code) {
    my $current = $*CWD;
    chdir $dir;
    if %resource{$code}<file> -> $fn {
        copy "$current/rc/resources/{$fn}", "./{$fn}"
    }
    dump-code ("$code%l<ext>") unless $quiet;
    check-dependencies("$code%l<ext>", $lang) if $deps;
    my @cmd = %resource{$code}<cmd> ?? |%resource{$code}<cmd> !! "%l<exe> $code%l<ext>\n";
    for @cmd -> $cmd {
        say "\nCommand line: {%c<cmd>}$cmd",%c<clr>;
        try shell $cmd;
    }
    chdir $current;
    say "\nDone $code";
}
 
sub pause {
    prompt "Press enter to procede:>";
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
 
sub uh-oh ($err) { put %c<warn>, "{'#' x 79}\n\n $err \n\n{'#' x 79}", %c<clr> }
 
multi check-dependencies ($fn, 'perl6') {
    my @use = $fn.IO.slurp.comb(/<?after $$ 'use '> \N+? <?before \h* ';'>/);
    if +@use {
        for @use -> $module {
            next if $module eq any('v6','nqp') or $module.contains('MONKEY');
            my $installed = $*REPO.resolve(CompUnit::DependencySpecification.new(:short-name($module)));
            shell("zef install $module") unless $installed;
        }
    }
}
 
multi check-dependencies  ($fn, $unknown) {
    note "Sorry, don't know how to handle dependancies for $unknown language."
};
 
multi load-lang ('perl6') { ( # Language specific variables. Adjust to suit.
    language => 'Perl_6', # language category name
    exe      => 'perl6',  # executable name to run perl6 in a shell
    ext      => '.p6',    # file extension for perl6 code
    dir      => 'perl6',  # directory to save tasks to
    header   => 'Perl 6', # header text
    # tags marking blocks of code - spaced out to placate wiki formatter
    # and to avoid getting tripped up when trying to run _this_ task
    tag => rx/<?after '<lang ' 'perl6' '>' > .*? <?before '</' 'lang>'>/,
) }
 
multi load-lang ('perl') { (
    language => 'Perl',
    exe      => 'perl',
    ext      => '.pl',
    dir      => 'perl',
    header   => 'Perl',
    tag => rx/<?after '<lang ' 'perl' '>' > .*? <?before '</' 'lang>'>/,
) }
 
multi load-lang ('python') { (
    language => 'Python',
    exe      => 'python',
    ext      => '.py',
    dir      => 'python',
    header   => 'Python',
    tag => rx/<?after '<lang ' 'python' '>' > .*? <?before '</' 'lang>'>/,
) }
 
multi load-lang ($unknown) { die "Sorry, don't know how to handle $unknown language." };
 
multi load-resources ($unknown) { () };
```

#### Output:
```
Retrieving tasks
1 Determine if a string is numeric
Getting code from: http://rosettacode.org/wiki/Determine_if_a_string_is_numeric#Perl_6

Testing Determine_if_a_string_is_numeric

Command line: perl6 Determine_if_a_string_is_numeric.p6
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
   <1 1 1>     False        False
        <>      True        False
       < >      True        False

Done Determine_if_a_string_is_numeric
===============================================================================
```


Or, if the full&#160;%resource hash is loaded it will automatically feed input parameters to tasks that require them:

**perl6 RC-run.p6 Lucky\_and\_even\_lucky\_numbers** -q


#### Output:
```
Retrieving tasks
1 Lucky_and_even_lucky_numbers
Getting code from: http://rosettacode.org/wiki/Lucky_and_even_lucky_numbers#Perl_6

Testing Lucky_and_even_lucky_numbers

Command line: perl6 Lucky_and_even_lucky_numbers.p6 20 , lucky

79

Command line: perl6 Lucky_and_even_lucky_numbers.p6 1 20

(1 3 7 9 13 15 21 25 31 33 37 43 49 51 63 67 69 73 75 79)

Command line: perl6 Lucky_and_even_lucky_numbers.p6 1 20 evenlucky

(2 4 6 10 12 18 20 22 26 34 36 42 44 50 52 54 58 68 70 76)

Command line: perl6 Lucky_and_even_lucky_numbers.p6 6000 -6100

(6009 6019 6031 6049 6055 6061 6079 6093)

Command line: perl6 Lucky_and_even_lucky_numbers.p6 6000 -6100 evenlucky

(6018 6020 6022 6026 6036 6038 6050 6058 6074 6090 6092)

Done Lucky_and_even_lucky_numbers
===============================================================================
```
