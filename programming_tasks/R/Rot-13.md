[1]: http://rosettacode.org/wiki/Rot-13

# [Rot-13][1]

```perl6
sub rot13 { $^s.trans: 'a..mn..z' => 'n..za..m', :ii }
 
multi MAIN ()        { print rot13 slurp }
multi MAIN (*@files) { print rot13 [~] map &slurp, @files }
```


This illustrates use of multi-dispatch to MAIN based on number of arguments.