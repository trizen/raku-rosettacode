[1]: https://rosettacode.org/wiki/Elementary_cellular_automaton/Infinite_length

# [Elementary cellular automaton/Infinite length][1]


This version, while it is *capable* of working with infinite length cellular automata,  makes the assumption that any cells which have not been explicitly examined are in a 'null' state, neither '0' or '1'. Further it makes the assumption that a null cell, on being examined, initially contains nothing (░). Otherwise it would take infinite time to calculate every row and would be exceptionally boring to watch.



Based heavily on the code from the [One-dimensional cellular automata](https://rosettacode.org/wiki/One-dimensional_cellular_automata#Raku) task. Example uses rule 90 (Sierpinski triangle).

```perl
class Automaton {
    has $.rule;
    has @.cells;
    has @.code = $!rule.fmt('%08b').flip.comb».Int;

    method gist { @!cells.map({+$_ ?? '▲' !! '░'}).join }

    method succ {
        self.new: :$!rule, :@!code, :cells(
            ' ',
            |@!code[
                    4 «*« @!cells.rotate(-1)
                »+« 2 «*« @!cells
                »+«       @!cells.rotate(1)
            ],
            ' '
        )
    }
}

my Automaton $a .= new: :rule(90), :cells(flat '010'.comb);

# display the first 20 rows
say $a++ for ^20;

# then calculate the other infinite number of rows, (may take a while)
$a++ for ^Inf;
```

#### Output:
```
░▲░
░▲░▲░
░▲░░░▲░
░▲░▲░▲░▲░
░▲░░░░░░░▲░
░▲░▲░░░░░▲░▲░
░▲░░░▲░░░▲░░░▲░
░▲░▲░▲░▲░▲░▲░▲░▲░
░▲░░░░░░░░░░░░░░░▲░
░▲░▲░░░░░░░░░░░░░▲░▲░
░▲░░░▲░░░░░░░░░░░▲░░░▲░
░▲░▲░▲░▲░░░░░░░░░▲░▲░▲░▲░
░▲░░░░░░░▲░░░░░░░▲░░░░░░░▲░
░▲░▲░░░░░▲░▲░░░░░▲░▲░░░░░▲░▲░
░▲░░░▲░░░▲░░░▲░░░▲░░░▲░░░▲░░░▲░
░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░▲░
░▲░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░▲░
░▲░▲░░░░░░░░░░░░░░░░░░░░░░░░░░░░░▲░▲░
░▲░░░▲░░░░░░░░░░░░░░░░░░░░░░░░░░░▲░░░▲░
░▲░▲░▲░▲░░░░░░░░░░░░░░░░░░░░░░░░░▲░▲░▲░▲░
^C
```
