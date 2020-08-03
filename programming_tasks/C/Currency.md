[1]: https://rosettacode.org/wiki/Currency

# [Currency][1]

No need for a special type in Perl 6, since the `Rat` type is used for normal fractions.
(In order to achieve imprecision, you have to explicitly use scientific notation,
or use the `Num` type, or calculate a result that requires a denominator in excess of `2 ** 64`. (There's no limit on the numerator.))

```raku
my @check = q:to/END/.lines.map: { [.split(/\s+/)] };
    Hamburger   5.50    4000000000000000
    Milkshake   2.86    2
    END
 
my $tax-rate = 0.0765;
 
my $fmt = "%-10s %8s %18s %22s\n";
 
printf $fmt, <Item Price Quantity Extension>;
 
my $subtotal = [+] @check.map: -> [$item,$price,$quant] {
    my $extension = $price * $quant;
    printf $fmt, $item, $price, $quant, fix2($extension);
    $extension;
}
 
printf $fmt, '', '', '', '-----------------';
printf $fmt, '', '', 'Subtotal ', $subtotal;
 
my $tax = ($subtotal * $tax-rate).round(0.01);
printf $fmt, '', '', 'Tax ', $tax;
 
my $total = $subtotal + $tax;
printf $fmt, '', '', 'Total ', $total;
 
# make up for lack of a Rat fixed-point printf format
sub fix2($x) { ($x + 0.001).subst(/ <?after \.\d\d> .* $ /, '') }
```

#### Output:
```
Item          Price           Quantity              Extension
Hamburger      5.50   4000000000000000   22000000000000000.00
Milkshake      2.86                  2                   5.72
                                            -----------------
                             Subtotal    22000000000000005.72
                                  Tax     1683000000000000.44
                                Total    23683000000000006.16
```