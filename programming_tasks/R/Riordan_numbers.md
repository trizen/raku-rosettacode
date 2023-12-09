[1]: https://rosettacode.org/wiki/Riordan_numbers

# [Riordan numbers][1]

```perl
use Lingua::EN::Numbers;

my @riordan = 1, 0, { state $n = 1; (++$n - 1) / ($n + 1) × (3 × $^a + 2 × $^b) } … *;

my $upto = 32;
say "First {$upto.&cardinal} Riordan numbers:\n" ~ @riordan[^$upto]».&comma».fmt("%17s").batch(4).join("\n") ~ "\n";

sub abr ($_) { .chars < 41 ?? $_ !! .substr(0,20) ~ '..' ~ .substr(*-20) ~ " ({.chars} digits)" }

say "The {.Int.&ordinal}: " ~ abr @riordan[$_ - 1] for 1e3, 1e4
```

#### Output:
```
First thirty-two Riordan numbers:
                1                 0                 1                 1
                3                 6                15                36
               91               232               603             1,585
            4,213            11,298            30,537            83,097
          227,475           625,992         1,730,787         4,805,595
       13,393,689        37,458,330       105,089,229       295,673,994
      834,086,421     2,358,641,376     6,684,761,125    18,985,057,351
   54,022,715,451   154,000,562,758   439,742,222,071 1,257,643,249,140

The one thousandth: 51077756867821111314..79942013897484633052 (472 digits)
The ten thousandth: 19927418577260688844..71395322020211157137 (4765 digits)
```
