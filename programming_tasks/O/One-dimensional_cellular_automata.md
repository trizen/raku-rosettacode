[1]: http://rosettacode.org/wiki/One-dimensional_cellular_automata

# [One-dimensional cellular automata][1]

We'll make a general algorithm capable of computing any cellular automata
as defined by [Stephen Wolfram](http://en.wikipedia.org/wiki/Stephen_Wolfram)'s
famous book *[A new kind of Science](http://en.wikipedia.org/wiki/A_new_kind_of_Science)*.
We will take the liberty of wrapping the array of cells
as it does not affect the result much
and it makes the implementation a lot easier.

```perl
class Automata {
    has ($.rule, @.cells);
    method gist { <| |>.join: @!cells.map({$_ ?? '#' !! ' '}).join }
    method code { $.rule.fmt("%08b").comb.reverse }
    method succ {
	self.new: :$.rule, :cells(
	    self.code[
                [Z+] 4 «*« @.cells.rotate(-1),
                     2 «*« @.cells,
                           @.cells.rotate(1)
	    ]
	)
    }
}
```


The rule proposed for this task is rule 0b01101000 = 104

```perl
my $size = 10;
my Automata $a .= new:
    :rule(104),
    :cells( 0 xx $size div 2, '111011010101'.comb, 0 xx $size div 2 );
say $a++ for ^10;
```

#### Output:
```
|     ### ## # # #     |
|     # ##### # #      |
|      ##   ## #       |
|      ##   ###        |
|      ##   # #        |
|      ##    #         |
|      ##              |
|      ##              |
|      ##              |
|      ##              |
```


Rule 104 is not particularly interesting so here is [Rule 90](http://en.wikipedia.org/wiki/Rule_90),
which shows a [Sierpinski Triangle](http://en.wikipedia.org/wiki/Sierpinski_Triangle).

```perl
my $size = 50;
my Automata $a .= new: :rule(90), :cells( 0 xx $size div 2, 1, 0 xx $size div 2 );
 
say $a++ for ^20;
```

#### Output:
```
|                         #                         |
|                        # #                        |
|                       #   #                       |
|                      # # # #                      |
|                     #       #                     |
|                    # #     # #                    |
|                   #   #   #   #                   |
|                  # # # # # # # #                  |
|                 #               #                 |
|                # #             # #                |
|               #   #           #   #               |
|              # # # #         # # # #              |
|             #       #       #       #             |
|            # #     # #     # #     # #            |
|           #   #   #   #   #   #   #   #           |
|          # # # # # # # # # # # # # # # #          |
|         #                               #         |
|        # #                             # #        |
|       #   #                           #   #       |
|      # # # #                         # # # #      |
```