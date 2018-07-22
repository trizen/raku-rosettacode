[1]: https://rosettacode.org/wiki/Retrieve_and_search_chat_history

# [Retrieve and search chat history][1]

No dependencies or third party libraries huh? How about no modules, no requires, no libraries, no imports at all. Implemented using a bare compiler, strictly with built-ins. (Which makes it kind-of verbose, but thems the trade-offs.)

```perl
my $needle = @*ARGS.shift // '';
my @haystack;
 
# 10 days before today, Zulu time
my $begin = DateTime.new(time).utc.earlier(:10days);
say "         Executed at: ", DateTime.new(time).utc;
say "Begin searching from: $begin";
 
# Today - 10 days through today
for $begin.Date .. DateTime.now.utc.Date -> $date {
 
    # connect to server, use a raw socket
    my $http = IO::Socket::INET.new(:host('tclers.tk'), :port(80));
 
    # request file
    $http.print: "GET /conferences/tcl/{$date}.tcl HTTP/1.0\n\n";
 
    # retrieve file
    my @page = $http.lines;
 
    # remove header
    @page.splice(0, 8);
 
    # concatenate multi-line entries to a single line
    while @page {
        if @page[0].substr(0, 13) ~~ m/^'m '\d\d\d\d'-'\d\d'-'\d\d'T'/ {
            @haystack.push: @page.shift;
        }
        else {
            @haystack.tail ~= '␤' ~ @page.shift;
        }
    }
 
    # close socket
    $http.close;
}
 
# ignore times before 10 days ago
@haystack.shift while @haystack[0].substr(2, 22) lt $begin.Str;
 
# print the first and last line of the haystack
say "First and last lines of the haystack:";
.say for |@haystack[0, *-1];
say "Needle: ", $needle;
say  '-' x 79;
 
# find and print needle lines
.say if .contains( $needle ) for @haystack;
```


Sample output using a needle of 'github.com'


#### Output:
```
         Executed at: 2017-05-05T02:13:55Z
Begin searching from: 2017-04-25T02:13:55Z
First and last lines of the haystack:
m 2017-04-25T02:25:17Z ijchain {*** TakinOver leaves}
m 2017-05-05T02:14:52Z {} {rmax has become available}
Needle: github.com
-------------------------------------------------------------------------------
m 2017-04-28T07:33:59Z ijchain {<Napier> https://github.com/Dash-OS/tcl-modules/blob/master/react-0.5.tm}
m 2017-04-28T07:35:40Z ijchain {<Napier> https://github.com/Dash-OS/tcl-modules/blob/master/react/reducer-0.5.tm}
m 2017-04-28T08:25:39Z ijchain {<Napier> https://github.com/Dash-OS/tcl-modules/blob/master/examples/react.md}
m 2017-04-28T08:45:01Z ijchain {<Napier> https://github.com/Dash-OS/tcl-modules/blob/master/examples/react.md}
m 2017-04-28T09:21:22Z ijchain {<Napier> decorators damnit! :-P https://github.com/Dash-OS/tcl-modules/blob/master/decorator-1.0.tm}
m 2017-04-28T15:42:28Z jima https://gist.github.com/antirez/6ca04dd191bdb82aad9fb241013e88a8
m 2017-04-28T16:20:22Z ijchain {<dbohdan> Look at what I've just made: https://github.com/dbohdan/ptjd}
m 2017-04-29T05:48:13Z ijchain {<dbohdan> The brackets, rather than braces, are there because the [catch] is used for metaprogramming: https://github.com/dbohdan/ptjd/blob/c0a77ecfb34c619e30ec7c5e9f448879d41282e2/tests.tcl#L48}
m 2017-04-29T14:49:08Z ijchain {<dbohdan> If you want CI for Windows builds, it should possible to set it up relatively easily with AppVeyor and the Git mirror at https://github.com/tcltk/tcl}
m 2017-04-29T14:50:58Z ijchain {<dbohdan> Here's an example: https://github.com/dbohdan/picol/blob/trunk/appveyor.yml}
m 2017-04-29T20:33:25Z ijchain {<Napier> https://github.com/pfultz2/Cloak/wiki/C-Preprocessor-tricks,-tips,-and-idioms}
m 2017-04-30T08:20:05Z ijchain {<bairui> AvL_42: fwiw, I like and use Apprentice: https://github.com/romainl/Apprentice}
m 2017-04-30T21:35:22Z ijchain {<Napier> https://github.com/Dash-OS/tcl-modules#cswitch-flags-----expr-script-}
m 2017-05-01T04:16:00Z ijchain {<bairui> avl42: https://github.com/dahu/DiffLine  --  see if that helps with your next long-line vimdiff. DISCLAIMER: It *shouldn't* eat your hard-drive, but buyer beware, check the code and test it on unimportant stuff first.}
m 2017-05-01T07:08:26Z ijchain {<dbohdan> Do take a look at this one, though: https://github.com/dbohdan/sqawk/blob/master/lib/tabulate.tcl#L74}
m 2017-05-01T07:09:28Z ijchain {<dbohdan> And at https://github.com/dbohdan/jimhttp/blob/master/arguments.tcl}
m 2017-05-01T08:14:05Z ijchain {<dbohdan> I've reimplemented part of tcltest recently (with pretty colors!), and I'm pretty with this solution: https://github.com/dbohdan/ptjd/blob/master/tests.tcl#L49}
m 2017-05-03T14:57:24Z ijchain {<dbohdan> I think Tcl is okay to pretty good for compilery things if you already know how to do them. For instance, I found writing this tokenizer in Tcl a breeze: https://github.com/dbohdan/jimhttp/blob/master/json.tcl#L380}
m 2017-05-04T16:19:55Z stu {rkeene, files Makefile.in, itzev and spoto.conf are spotoconf: https://github.com/aryler/Tclarc4random/}
m 2017-05-04T20:26:28Z dgp https://github.com/flightaware/Tcl-bounties/issues/25
```