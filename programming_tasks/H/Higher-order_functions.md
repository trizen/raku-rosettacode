[1]: http://rosettacode.org/wiki/Higher-order_functions

# [Higher-order functions][1]

The best type to use for the parameter of a higher-order function is `Callable` (implied by the `&amp;` sigil), a role common to all function-like objects. For an example of defining and calling a second-order function, see [Functional Composition](http://rosettacode.org/wiki/Functional_Composition#Perl_6).



Convenient syntax is provided for anonymous functions,
either a bare block, or a parameterized block introduced with `-&gt;`, which serves as a "lambda":

```perl
sub twice(&todo) {
   todo(); todo(); # declaring &todo also defines bare function
}
twice { say "Boing!" }
# output:
# Boing!
# Boing!
 
sub twice-with-param(&todo) {
    todo(0); todo(1);
}
twice-with-param -> $time {
   say "{$time+1}: Hello!"
}
# output:
# 1: Hello!
# 2: Hello!
```