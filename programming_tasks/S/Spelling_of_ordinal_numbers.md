[1]: https://rosettacode.org/wiki/Spelling_of_ordinal_numbers

# [Spelling of ordinal numbers][1]

This would be pretty simple to implement from scratch; it would be straightforward to do a minor modification of the [ Number names](https://rosettacode.org/wiki/Number_names#Perl_6) task code. Much simpler to just use the Lingua::EN::Numbers::Cardinal module from the Perl 6 ecosystem though. It easily handles ordinal numbers even though that is not its primary focus.



We need to be slightly careful of terminology. In Perl 6, 123, 00123.0, &amp; 1.23e2 are not all integers. They are respectively an Int (integer), a Rat (rational number) and a Num (floating point number). For this task it doesn't much matter as the ordinal routine coerces its argument to an Int, but to Perl 6 they are different things. We can further abuse allomorphic types for some somewhat non-intuitive results as well.



It is not really clear what is meant by "Write a driver and a function...". Well, the function part is clear enough; driver not so much. Perhaps this will suffice.

```raku
use Lingua::EN::Numbers::Cardinal;
 
printf( "\%16s : %s\n", $_, ordinal($_) ) for
 
# Required tests
|<1 2 3 4 5 11 65 100 101 272 23456 8007006005004003>,
 
# Optional tests
|<123 00123.0 1.23e2 123+0i 0b1111011 0o173 0x7B 861/7>;
```

#### Output:
```
               1 : first
               2 : second
               3 : third
               4 : fourth
               5 : fifth
              11 : eleventh
              65 : sixty-fifth
             100 : one hundredth
             101 : one hundred first
             272 : two hundred seventy-second
           23456 : twenty-three thousand, four hundred fifty-sixth
8007006005004003 : eight quadrillion, seven trillion, six billion, five million, four thousand third
             123 : one hundred twenty-third
         00123.0 : one hundred twenty-third
          1.23e2 : one hundred twenty-third
          123+0i : one hundred twenty-third
       0b1111011 : one hundred twenty-third
           0o173 : one hundred twenty-third
            0x7B : one hundred twenty-third
           861/7 : one hundred twenty-third
```