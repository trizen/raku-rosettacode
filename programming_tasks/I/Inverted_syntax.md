[1]: https://rosettacode.org/wiki/Inverted_syntax

# [Inverted syntax][1]

Like all Perls, Perl 6 has statement modifiers:

```raku
if $guess == 6 { say "Wow! Lucky Guess!" }          # Traditional
say 'Wow! Lucky Guess!' if $guess == 6;             # Inverted
unless $guess == 6 { say "Huh! You Guessed Rong!" } # Traditional
say 'Huh! You Guessed Rong!' unless $guess == 6;    # Inverted
```


Perl also inverts the syntax of loops:

```raku
while $i { --$i }
--$i while $i;
 
until $x > 10 { $x++ }
$x++ until $x > 10;
 
for 1..10 { .say if $_ %% 2 }
.say if $_ %% 2 for 1..10;  # list comprehension
```


Perl 6 has a system of metaoperators that modify the characteristics of normal operators. Among these is the `R` metaoperator, which is able to reverse the arguments of most infix operators (including user-defined ones).
So a reversed assignment is easy to write:

```raku
42 R= $_; say $_;   # prints 42
```


Since, much like list operators, assignment loosens the precedence of the following expression to allow comma lists, reverse assignment of a list requires parens where the normal assignment would not:

```raku
my @a = 1,2,3;
(1,2,3) R= my @a;
```


However, generally in that case you'd use a feed operator anyway, which is like an object pipe, but unlike Unix pipes works in either direction:

```raku
my @a <== 1,2,3;
1,2,3 ==> my @a;
```


We think this is much more readable than a reversed assignment.



One other interesting inversion is the ability to put the conditional of a repeat loop at either end, with identical test-after semantics:

```raku
repeat {
    $_ = prompt "Gimme a number: ";
} until /^\d+$/;
 
repeat until /^\d+$/ {
    $_ = prompt "Gimme a number: ";
}
```


This might seem relatively useless, but it allows a variable to be declared in the conditional that isn't actually set until the loop body:

```raku
repeat until my $answer ~~ 42 {
    $answer = prompt "Gimme an answer: ";
}
```


This would require a prior declaration (and two extra semicolons, horrors)
if written in the non-inverted form with the conditional at the bottom:

```raku
my $answer;
repeat {
    $answer = prompt "Gimme an answer: ";
} until $answer ~~ 42;
```


You can't just put the `my` on the `$answer` in the block because the conditional is outside the scope of the block, and would not see the declaration.