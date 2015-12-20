[1]: http://rosettacode.org/wiki/Top_rank_per_group

# [Top rank per group][1]

We use tab-separated fields here from a heredoc; <tt>q:to/---/</tt> begins the heredoc. The <tt>Z=&gt;</tt> operator zips two lists into a list of pairs.
In <tt>MAIN</tt>, the <tt>classify</tt> method generates pairs where each key is a different department, and each value all the entries in that department. We then sort the pairs and process each department separately. Within each department, we sort on salary (negated to reverse the order). The last statement is essentially a list comprehension that uses a slice subscript with the <tt>^</tt> "up to" operator to take the first N elements of the sorted employee list. The <tt>:v</tt> modifier returns only valid values. The <tt>.&lt;Name&gt;</tt> form is a slice hash subscript with literals strings. That in turn is just the subscript form of the <tt>&lt;...&gt;</tt> ("quote words") form, which is more familar to Perl 5 programmers as
<tt>qw/.../</tt>. We used that form earlier to label the initial data set.



This program also makes heavy use of method calls that start with dot. In Perl&#160;6 this means a method call on the current topic, <tt>$\_</tt>, which is automatically set by any <tt>for</tt> or <tt>map</tt> construct that doesn't declare an explicit formal parameter on its closure.

```perl6
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
      $line.split(/ \t+ /)
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