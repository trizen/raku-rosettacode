[1]: http://rosettacode.org/wiki/Variables

# [Variables][1]

Much of what is true for Perl 5 is also true for Perl 6. Some exceptions:



There are no typeglobs in Perl 6.



Assigning an array to a scalar variable now makes that scalar variable a reference to the array:

```perl6
 
my @y = <A B C D>; # Array of strings 'A', 'B', 'C', and 'D'
say @y[2]; # the @-sigil requires the container to implement the role Positional
@y[1,2] = 'x','y'; # that's where subscripts and many other things come from
say @y; # OUTPUT«[A x y D]␤» # we start to count at 0 btw.
 
my $x = @y; # $x is now a reference for the array @y
 
say $x[1]; # prints 'x' followed by a newline character
 
my Int $with-type-check; # type checks are enforced by the compiler
 
my Int:D $defined-i = 10; # definedness can also be asked for and default values are required in that case
 
my Int:D $after-midnight where * > 24 = 25; # SQL is fun and so is Perl 6
 
my \bad = 'good'; # if you don't like sigils
say bad; # you don't have to use them
say "this is quite bad"; # but then string interpolation
say "this is quite {bad}" # becomes more wordy
 
```


Laziness is a big topic in Perl 6. Sometimes Perl programmers are so lazy, they can't even be bothered with giving [variables names](http://design.perl6.org/S02.html#Names\_and\_Variables).

```perl6
say ++$; # this is an anonymous state variable
say ++$; # this is a different anonymous state variable, prefix:<++> forces it into numerical context and defaults it to 0
say $+=2 for 1..10 # here we do something useful with another anonymous variable
 
sub foo { $^a * $^b } # for positional arguments we often can't be bothered to declare them or to give them fancy names
say foo 3, 4;
```

#### Output:
```
1
1
2
4
6
12
```


(Includes code modified from [http://design.perl6.org/S02.html#Built-In\_Data\_Types](http://design.perl6.org/S02.html#Built-In\_Data\_Types). See this reference for more details.)