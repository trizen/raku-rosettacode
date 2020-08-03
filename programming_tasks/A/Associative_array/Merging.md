[1]: https://rosettacode.org/wiki/Associative_array/Merging

# [Associative array/Merging][1]

I must say I somewhat disagree with the terminology. The requested operation is an update not a merge. Demonstrate both an update and a merge. Associative arrays are commonly called hashes in Perl 6.

```perl
# Show original hashes
say my %base   = :name('Rocket Skates'), :price<12.75>, :color<yellow>;
say my %update = :price<15.25>, :color<red>, :year<1974>;
 
# Need to assign to anonymous hash to get the desired results and avoid mutating
# TIMTOWTDI
say "\nUpdate:\n", join "\n", sort %=%base, %update;
# Same
say "\nUpdate:\n", {%base, %update}.sort.join: "\n";
 
say "\nMerge:\n", join "\n", sort ((%=%base).push: %update)».join: ', ';
# Same
say "\nMerge:\n", ({%base}.push: %update)».join(', ').sort.join: "\n";
 
# Demonstrate unmutated hashes
say "\n", %base, "\n", %update;
```

#### Output:
```
{color => yellow, name => Rocket Skates, price => 12.75}
{color => red, price => 15.25, year => 1974}

Update:
color   red
name    Rocket Skates
price   15.25
year    1974

Update:
color   red
name    Rocket Skates
price   15.25
year    1974

Merge:
color   yellow, red
name    Rocket Skates
price   12.75, 15.25
year    1974

Merge:
color   yellow, red
name    Rocket Skates
price   12.75, 15.25
year    1974

{color => yellow, name => Rocket Skates, price => 12.75}
{color => red, price => 15.25, year => 1974}
```
