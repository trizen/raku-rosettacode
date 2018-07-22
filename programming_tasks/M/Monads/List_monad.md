[1]: https://rosettacode.org/wiki/Monads/List_monad

# [Monads/List monad][1]

Perl&#160;6 does not have Monad types built in but they can be emulated/implemented without a great deal of difficulty. List Monads especially are of questionable utility in Perl&#160;6. Most item types and Listy types have a Cool role in Perl&#160;6. (Cool being a play on the slang term "cool" as in: "That's cool with me." (That's ok with me). So Ints are pretty much treated like one item lists for operators that work with lists. ("I work on a list." "Here's an Int." "Ok, that's cool.") Explicitly wrapping an Int into a List is worse than useless. It won't do anything Perl&#160;6 can't do natively, and will likely **remove** some functionality that it would normally have. That being said, just because it is a bad idea (in Perl&#160;6) doesn't mean it can't be done.



In Perl&#160;6, bind is essentially map. I'll shadow map here but again, it **removes** capability, not adds it. Perl&#160;6 also provided "hyper" operators which will descend into data structures and apply an operator / function to each member of that data structure.



Here's a simple, if contrived example. take the numbers from 0 to 9, add 3 to each, find the divisors of those sums and print the list of divisors for each sum... in base 2. Again, a bind function was implemented but it is more limited than if we just used map directly. The built in map method will work with either items or lists, here we need to implement a multi sub to handle either.



The \* in the bind blocks are typically referred to as "whatever"; whatever + 3 etc. The guillemot (») is the hyper operator; descend into the data structure and apply the following operator/function to each member.

```perl
multi bind (@list, &code) { @list.map: &code };
 
multi bind ($item, &code) { $item.&code };
 
sub divisors (Int $int) { gather for 1 .. $int { .take if $int %% $_ } }
 
put join "\n", (flat ^10).&bind(* + 3).&bind(*.&divisors)».&bind(*.base: 2);
```

#### Output:
```
1 11
1 10 100
1 101
1 10 11 110
1 111
1 10 100 1000
1 11 1001
1 10 101 1010
1 1011
1 10 11 100 110 1100
```