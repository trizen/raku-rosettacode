[1]: https://rosettacode.org/wiki/Elementary_cellular_automaton

# [Elementary cellular automaton][1]





Using the `Automaton` class defined at [One-dimensional_cellular_automata#Raku](https://rosettacode.org/wiki/One-dimensional_cellular_automata#Raku):

```perl
class Automaton {
    has $.rule;
    has @.cells;
    has @.code = $!rule.fmt('%08b').flip.comb».Int;
 
    method gist { "|{ @!cells.map({+$_ ?? '#' !! ' '}).join }|" }
 
    method succ {
        self.new: :$!rule, :@!code, :cells( 
            @!code[
                    4 «*« @!cells.rotate(-1)
                »+« 2 «*« @!cells
                »+«       @!cells.rotate(1)
            ]
        )
    }
}

my @padding = 0 xx 10;

my Automaton $a .= new:
    :rule(30),
    :cells(flat @padding, 1, @padding);

say $a++ for ^10;
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
