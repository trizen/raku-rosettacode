[1]: https://rosettacode.org/wiki/Top_rank_per_group

# [Top rank per group][1]


We use whitespace-separated fields here from a heredoc; `q:to/---/` begins the heredoc. The `Z=>` operator zips two lists into a list of pairs.
In `MAIN`, the `classify` method generates pairs where each key is a different department, and each value all the entries in that department. We then sort the pairs and process each department separately.  Within each department, we sort on salary (negated to reverse the order).  The last statement is essentially a list comprehension that uses a slice subscript with the `^` "up to" operator to take the first N elements of the sorted employee list. The `:v` modifier returns only valid values. The `.<Name>` form is a slice hash subscript with literals strings.  That in turn is just the subscript form of the `<...>` ("quote words") form, which is more familar to Perl 5 programmers as
`qw/.../`.  We used that form earlier to label the initial data set.



This program also makes heavy use of method calls that start with dot.  In Raku this means a method call on the current topic, `$_`, which is automatically set by any `for` or `map` construct that doesn't declare an explicit formal parameter on its closure.

```perl
my @data = do for q:to/---/.lines -> $line {
        E10297  32000   D101    Tyler Bennett
        E21437  47000   D050    John Rappl
        E00127  53500   D101    George Woltman
        E63535  18000   D202    Adam Smith
        E39876  27800   D202    Claire Buckman
        E04242  41500   D101    David McClellan
        E01234  49500   D202    Rich Holcomb
        E41298  21900   D050    Nathan Adams
        E43128  15900   D101    Richard Potter
        E27002  19250   D202    David Motsinger
        E03033  27000   D101    Tim Sampair
        E10001  57000   D190    Kim Arlich
        E16398  29900   D190    Timothy Grove
        ---

  $%( < Id      Salary  Dept    Name >
      Z=>
      $line.split(/ \s\s+ /)
    )
}

sub MAIN(Int $N = 3) {
    for @data.classify({ .<Dept> }).sort».value {
        my @es = .sort: { -.<Salary> }
        say '' if (state $bline)++;
        say .< Dept Id Salary Name > for @es[^$N]:v;
    }
}
```

#### Output:
```
D050 E21437 47000 John Rappl
D050 E41298 21900 Nathan Adams

D101 E00127 53500 George Woltman
D101 E04242 41500 David McClellan
D101 E10297 32000 Tyler Bennett

D190 E10001 57000 Kim Arlich
D190 E16398 29900 Timothy Grove

D202 E01234 49500 Rich Holcomb
D202 E39876 27800 Claire Buckman
D202 E27002 19250 David Motsinger
```
