[1]: http://rosettacode.org/wiki/Elementary_cellular_automaton

# [Elementary cellular automaton][1]

```perl6
class Automaton {
    has ($.rule, @.cells);
    method gist { <| |>.join: @!cells.map({+$_ ?? '#' !! ' '}).join }
    method code { $!rule.fmt("%08b").flip.comb }
    method succ {
        self.new: :$!rule, :cells(
            self.code[
                   (4 X* @!cells.rotate(-1))
                Z+ (2 X* @!cells)
                Z+       @!cells.rotate(1)
            ]
        )
    }
}
 
my $size = 10;
my Automaton $a .= new:
    :rule(30),
    :cells( flat 0 xx $size, 1, 0 xx $size );
say $a++ for ^$size;
```

#### Output:
```
|          #          |
|         ###         |
|        ##  #        |
|       ## ####       |
|      ##  #   #      |
|     ## #### ###     |
|    ##  #    #  #    |
|   ## ####  ######   |
|  ##  #   ###     #  |
| ## #### ##  #   ### |
```


Unfortunately that version is somewhat slow due to the (as yet) unoptimized vector operations, as well as rebuilding the code every iteration. The following version runs about ten times faster and produces the same output:

```perl6
class Automaton {
    has $.rule;
    has @.cells;
    has @.code;
 
    method new (|args) { self.bless(|args).init }
    method init { @!code ||= $!rule.fmt("%08b").flip.comb».Int; self; }
 
    method gist { <| |>.join: @!cells.map({$_ ?? '#' !! ' '}).join }
    method succ {
        my \size = +@!cells;
        self.new: :$!rule, :@!code, :cells(
            @!code[
                for ^size -> \i {
                    4 * @!cells[(i - 1) % size] +
                    2 * @!cells[i] +
                        @!cells[(i+1) % size];
                }
            ]
        );
    }
}
 
my $size = 10;
my Automaton $a .= new:
    :rule(30),
    :cells( 0 xx $size, 1, 0 xx $size );
say $a++ for ^$size;
```