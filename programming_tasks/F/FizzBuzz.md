[1]: https://rosettacode.org/wiki/FizzBuzz

# [FizzBuzz][1]

Most straightforwardly:

```raku
for 1 .. 100 {
    when $_ %% (3 & 5) { say 'FizzBuzz'; }
    when $_ %% 3       { say 'Fizz'; }
    when $_ %% 5       { say 'Buzz'; }
    default            { .say; }
}
```


Or abusing multi subs:

```raku
multi sub fizzbuzz(Int $ where * %% 15) { 'FizzBuzz' }
multi sub fizzbuzz(Int $ where * %%  5) { 'Buzz' }
multi sub fizzbuzz(Int $ where * %%  3) { 'Fizz' }
multi sub fizzbuzz(Int $number        ) { $number }
(1 .. 100)».&fizzbuzz.say;
```


Or abusing list metaoperators:

```raku
[1..100].map({[~] ($_%%3, $_%%5) »||» "" Z&& <fizz buzz> or $_ })».say
```


Concisely (readable):

```raku
say 'Fizz' x $_ %% 3 ~ 'Buzz' x $_ %% 5 || $_ for 1 .. 100;
```


Shortest FizzBuzz to date:

```raku
say "Fizz"x$_%%3~"Buzz"x$_%%5||$_ for 1..100
```


And here's an implementation that never checks for divisibility:

```raku
.say for
    (
      (flat ('' xx 2, 'Fizz') xx *)
      Z~
      (flat ('' xx 4, 'Buzz') xx *)
    )
    Z||
    1 .. 100;
```