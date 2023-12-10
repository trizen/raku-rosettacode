[1]: https://rosettacode.org/wiki/Execute_HQ9%2B

# [Execute HQ9+][1]





The spec is kind of vague about how to do error handling... and whether white space is significant... and how the accumulator should be accessed... and pretty much everything else too.

```perl
class HQ9Interpreter {
    has @!code;
    has $!accumulator;
    has $!pointer;
 
    method run ($code) {
        @!code = $code.comb;
        $!accumulator = 0;
        $!pointer = 0;
        while $!pointer < @!code {
            given @!code[$!pointer].lc {
                when 'h' { say 'Hello world!' }
                when 'q' { say @!code }
                when '9' { bob(99) }
                when '+' { $!accumulator++ }
                default  { note "Syntax error: Unknown command \"{@!code[$!pointer]}\"" }
            }
	    $!pointer++;
        }
    }
    sub bob ($beer is copy) {
        sub what  { "{$beer??$beer!!'No more'} bottle{$beer-1??'s'!!''} of beer" };
        sub where { 'on the wall' };
        sub drink { $beer--; "Take one down, pass it around," }
        while $beer {
            .say for "&what() &where(),", "&what()!",
                     "&drink()", "&what() &where()!", ''
        }
    }
}

# Feed it a command string:

my $hq9 = HQ9Interpreter.new;
$hq9.run("hHq+++Qq");
say '';
$hq9.run("Jhq.k+hQ");
```


Output:


```
Hello world!
Hello world!
hHq+++Qq
hHq+++Qq
hHq+++Qq

Syntax error: Unknown command "J"
Hello world!
Jhq.k+hQ
Syntax error: Unknown command "."
Syntax error: Unknown command "k"
Hello world!
Jhq.k+hQ
```


Or start a REPL (Read Execute Print Loop) and interact at the command line:

```perl
my $hq9 = HQ9Interpreter.new;
while 1 {
    my $in = prompt('HQ9+>').chomp;
    last unless $in.chars;
    $hq9.run($in)
}
```
