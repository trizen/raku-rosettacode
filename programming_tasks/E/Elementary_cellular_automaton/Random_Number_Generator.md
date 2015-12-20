[1]: http://rosettacode.org/wiki/Elementary_cellular_automaton/Random_Number_Generator

# [Elementary cellular automaton/Random Number Generator][1]

```perl6
my Automaton $a .= new: :rule(30), :cells( 1, 0 xx 100 );
 
say :2[$a++.cells[0] xx 8] xx 10;
```

#### Output:
```
220 197 147 174 117 97 149 171 240 241
```