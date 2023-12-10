[1]: https://rosettacode.org/wiki/Spelling_of_ordinal_numbers

# [Spelling of ordinal numbers][1]





Rakudo version 2019.07.1 is updated to Unicode version 12.1. Unicode version 12.0 introduced some new numeric digits, which changed the output here a bit. This will *work* with earlier versions of Rakudo, but will yield slightly different results.



This would be pretty simple to implement from scratch; it would be straightforward to do a minor modification of the [ Number names](https://rosettacode.org/wiki/Number_names#Raku) task code. Much simpler to just use the Lingua::EN::Numbers module from the Raku ecosystem though. It will easily handles ordinal number conversions.



We need to be slightly careful of terminology. In Raku, 123, 00123.0, &amp; 1.23e2 are not all integers. They are respectively an Int (integer), a Rat (rational number) and a Num (floating point number). (The fourth numeric is a Complex) For this task it doesn't much matter as the ordinal routine coerces its argument to an Int, but to Raku they are different things. We can further abuse allomorphic and String types and role mixins for some somewhat non-intuitive results as well.



Note that the different allomorphic integer forms of 123 are evaluated on *use*, not on *assignment*. They can be passed around in parameters, but until they are used numerically, they retain their stringy characteristics and are distinctive, determinable through introspection. The numerics are evaluated on assignment, hence the stringified output not exactly matching the input format. The mixin role returns different things depending on what context you evaluate it under. When evaluated as a string it is 17, numerically, it is 123.



Raku uses Unicode natively. If a glyph has a Unicode 'Numeric Digit' (&lt;:Nd&gt;) property, it is treated as a numeric digit, and may be used as one.



It is not really clear what is meant by "Write a driver and a function...". Well, the function part is clear enough; driver not so much. Perhaps this will suffice.

```perl
use Lingua::EN::Numbers;

# The task
+$_ ?? printf( "Type: \%-14s %16s : %s\n", .^name, $_, .&ordinal ) !! say "\n$_:" for

# Testing
'Required tests',
1, 2, 3, 4, 5, 11, 65, 100, 101, 272, 23456, 8007006005004003,

'Optional tests - different forms of 123',
'Numerics',
123, 00123.0, 1.23e2, 123+0i,

'Allomorphs',
|<123 1_2_3 00123.0 1.23e2 123+0i 0b1111011 0o173 0x7B 861/7>,

'Numeric Strings',
|'1_2_3 00123.0 1.23e2 123+0i 0b1111011 0o173 0x7B 861/7'.words,

'Unicode Numeric Strings',
# (Only using groups of digits from the same Unicode block. Technically,
# digits from any block could be combined with digits from any other block.)
|(^0x1FFFF).grep( { .chr ~~ /<:Nd>/ and .unival == 1|2|3 }).rotor(3)».chr».join,

'Role Mixin',
'17' but 123;
```

#### Output:
```
Required tests:
Type: Int                           1 : first
Type: Int                           2 : second
Type: Int                           3 : third
Type: Int                           4 : fourth
Type: Int                           5 : fifth
Type: Int                          11 : eleventh
Type: Int                          65 : sixty-fifth
Type: Int                         100 : one hundredth
Type: Int                         101 : one hundred first
Type: Int                         272 : two hundred seventy-second
Type: Int                       23456 : twenty-three thousand, four hundred fifty-sixth
Type: Int            8007006005004003 : eight quadrillion, seven trillion, six billion, five million, four thousand third

Optional tests - different forms of 123:

Numerics:
Type: Int                         123 : one hundred twenty-third
Type: Rat                         123 : one hundred twenty-third
Type: Num                         123 : one hundred twenty-third
Type: Complex                  123+0i : one hundred twenty-third

Allomorphs:
Type: IntStr                      123 : one hundred twenty-third
Type: IntStr                    1_2_3 : one hundred twenty-third
Type: RatStr                  00123.0 : one hundred twenty-third
Type: NumStr                   1.23e2 : one hundred twenty-third
Type: ComplexStr               123+0i : one hundred twenty-third
Type: IntStr                0b1111011 : one hundred twenty-third
Type: IntStr                    0o173 : one hundred twenty-third
Type: IntStr                     0x7B : one hundred twenty-third
Type: RatStr                    861/7 : one hundred twenty-third

Numeric Strings:
Type: Str                       1_2_3 : one hundred twenty-third
Type: Str                     00123.0 : one hundred twenty-third
Type: Str                      1.23e2 : one hundred twenty-third
Type: Str                      123+0i : one hundred twenty-third
Type: Str                   0b1111011 : one hundred twenty-third
Type: Str                       0o173 : one hundred twenty-third
Type: Str                        0x7B : one hundred twenty-third
Type: Str                       861/7 : one hundred twenty-third

Unicode Numeric Strings:
Type: Str                         123 : one hundred twenty-third
Type: Str                         ١٢٣ : one hundred twenty-third
Type: Str                         ۱۲۳ : one hundred twenty-third
Type: Str                         ߁߂߃ : one hundred twenty-third
Type: Str                         १२३ : one hundred twenty-third
Type: Str                         ১২৩ : one hundred twenty-third
Type: Str                         ੧੨੩ : one hundred twenty-third
Type: Str                         ૧૨૩ : one hundred twenty-third
Type: Str                         ୧୨୩ : one hundred twenty-third
Type: Str                         ௧௨௩ : one hundred twenty-third
Type: Str                         ౧౨౩ : one hundred twenty-third
Type: Str                         ೧೨೩ : one hundred twenty-third
Type: Str                         ൧൨൩ : one hundred twenty-third
Type: Str                         ෧෨෩ : one hundred twenty-third
Type: Str                         ๑๒๓ : one hundred twenty-third
Type: Str                         ໑໒໓ : one hundred twenty-third
Type: Str                         ༡༢༣ : one hundred twenty-third
Type: Str                         ၁၂၃ : one hundred twenty-third
Type: Str                         ႑႒႓ : one hundred twenty-third
Type: Str                         ១២៣ : one hundred twenty-third
Type: Str                         ᠑᠒᠓ : one hundred twenty-third
Type: Str                         ᥇᥈᥉ : one hundred twenty-third
Type: Str                         ᧑᧒᧓ : one hundred twenty-third
Type: Str                         ᪁᪂᪃ : one hundred twenty-third
Type: Str                         ᪑᪒᪓ : one hundred twenty-third
Type: Str                         ᭑᭒᭓ : one hundred twenty-third
Type: Str                         ᮱᮲᮳ : one hundred twenty-third
Type: Str                         ᱁᱂᱃ : one hundred twenty-third
Type: Str                         ᱑᱒᱓ : one hundred twenty-third
Type: Str                         ꘡꘢꘣ : one hundred twenty-third
Type: Str                         ꣑꣒꣓ : one hundred twenty-third
Type: Str                         ꤁꤂꤃ : one hundred twenty-third
Type: Str                         ꧑꧒꧓ : one hundred twenty-third
Type: Str                         ꧱꧲꧳ : one hundred twenty-third
Type: Str                         ꩑꩒꩓ : one hundred twenty-third
Type: Str                         ꯱꯲꯳ : one hundred twenty-third
Type: Str                         １２３ : one hundred twenty-third
Type: Str                         𐒡𐒢𐒣 : one hundred twenty-third
Type: Str                         𐴱𐴲𐴳 : one hundred twenty-third
Type: Str                         𑁧𑁨𑁩 : one hundred twenty-third
Type: Str                         𑃱𑃲𑃳 : one hundred twenty-third
Type: Str                         𑄷𑄸𑄹 : one hundred twenty-third
Type: Str                         𑇑𑇒𑇓 : one hundred twenty-third
Type: Str                         𑋱𑋲𑋳 : one hundred twenty-third
Type: Str                         𑑑𑑒𑑓 : one hundred twenty-third
Type: Str                         𑓑𑓒𑓓 : one hundred twenty-third
Type: Str                         𑙑𑙒𑙓 : one hundred twenty-third
Type: Str                         𑛁𑛂𑛃 : one hundred twenty-third
Type: Str                         𑜱𑜲𑜳 : one hundred twenty-third
Type: Str                         𑣡𑣢𑣣 : one hundred twenty-third
Type: Str                         𑱑𑱒𑱓 : one hundred twenty-third
Type: Str                         𑵑𑵒𑵓 : one hundred twenty-third
Type: Str                         𑶡𑶢𑶣 : one hundred twenty-third
Type: Str                         𖩡𖩢𖩣 : one hundred twenty-third
Type: Str                         𖭑𖭒𖭓 : one hundred twenty-third
Type: Str                         𝟏𝟐𝟑 : one hundred twenty-third
Type: Str                         𝟙𝟚𝟛 : one hundred twenty-third
Type: Str                         𝟣𝟤𝟥 : one hundred twenty-third
Type: Str                         𝟭𝟮𝟯 : one hundred twenty-third
Type: Str                         𝟷𝟸𝟹 : one hundred twenty-third
Type: Str                         𞥑𞥒𞥓 : one hundred twenty-third

Role Mixin:
Type: Str+{<anon|1>}               17 : one hundred twenty-third
```
