[1]: https://rosettacode.org/wiki/Monads/Writer_monad

# [Monads/Writer monad][1]

Basic semantic borrowed from the Monads/List monad entry

```raku
# 20200508 Raku programming solution
 
class Writer { has Numeric $.value ; has Str $.log }
 
sub Bind (Writer \v, &code) {
   my \n = v.value.&code;
   Writer.new: value => n.value, log => v.log ~ n.log
};
 
sub Unit(\v, \s) { Writer.new: value=>v, log=>sprintf "%-17s: %.12f\n",s,v}
 
sub root(\v) { Unit v.sqrt, "Took square root" }
 
sub addOne(\v) { Unit v+1, "Added one" }
 
sub half(\v) { Unit v/2, "Divided by two" }
 
say Unit(5, "Initial value").&Bind(&root).&Bind(&addOne).&Bind(&half).log;
```

#### Output:
```
Initial value    : 5.000000000000
Took square root : 2.236067977500
Added one        : 3.236067977500
Divided by two   : 1.618033988750
```
