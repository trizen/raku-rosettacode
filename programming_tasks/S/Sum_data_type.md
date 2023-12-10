[1]: https://rosettacode.org/wiki/Sum_data_type

# [Sum data type][1]





Raku doesn't really have Sum Types as a formal data structure but they can be emulated with enums and switches or multi-dispatch. Note that in this case, feeding the dispatcher an incorrect value results in a hard fault; it doesn't just dispatch to the default. Of course those rules can be relaxed or made more restrictive depending on your particular use case.

```perl
enum Traffic-Signal < Red Yellow Green Blue >;

sub message (Traffic-Signal $light) {
    with $light {
        when Red    { 'Stop!'                                       }
        when Yellow { 'Speed Up!'                                   }
        when Green  { 'Go! Go! Go!'                                 }
        when Blue   { 'Wait a minute, How did we end up in Japan?!' }
        default     { 'Whut?'                                       }
    }
}

my \Pink = 'A Happy Balloon';


for Red, Green, Blue, Pink -> $signal {
    say message $signal;
}
```

#### Output:
```
Stop!
Go! Go! Go!
Wait a minute, How did we end up in Japan?!
Type check failed in binding to parameter '$light'; expected Traffic-Signal but got Str ("A Happy Balloon")
```
