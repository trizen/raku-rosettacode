[1]: https://rosettacode.org/wiki/Magnanimous_numbers

# [Magnanimous numbers][1]

```perl
my @magnanimous = lazy flat ^10, (10 .. 1001).hyper.map( {
    my $last;
    (1 .. .chars - 1).map: -> \c { ++$last and last unless (.substr(0,c) + .substr(c)).is-prime }
    next if $last;
    $_
} ),
 
(1002 .. ∞).hyper.map: {
    # optimization for numbers > 1001; First and last digit can not both be even or both be odd
    next if (.substr(0,1) % 2 + .substr(*-1) % 2) %% 2;
    my $last;
    (1 .. .chars - 1).map: -> \c { ++$last and last unless (.substr(0,c) + .substr(c)).is-prime }
    next if $last;
    $_
}
 
put 'First 45 magnanimous numbers';
put @magnanimous[^45]».fmt('%3d').batch(15).join: "\n";
 
put "\n241st through 250th magnanimous numbers";
put @magnanimous[240..249];
 
put "\n391st through 400th magnanimous numbers";
put @magnanimous[390..399];
```

#### Output:
```
First 45 magnanimous numbers
  0   1   2   3   4   5   6   7   8   9  11  12  14  16  20
 21  23  25  29  30  32  34  38  41  43  47  49  50  52  56
 58  61  65  67  70  74  76  83  85  89  92  94  98 101 110

241st through 250th magnanimous numbers
17992 19972 20209 20261 20861 22061 22201 22801 22885 24407

391st through 400th magnanimous numbers
486685 488489 515116 533176 551558 559952 595592 595598 600881 602081
```
