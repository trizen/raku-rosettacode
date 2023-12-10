[1]: https://rosettacode.org/wiki/Gapful_numbers

# [Gapful numbers][1]





Also test starting on a number that *doesn't* start with 1. Required to have titles, may as well make 'em noble.&#160;:-)

```perl
use Lingua::EN::Numbers;

for (1e2, 30, 1e6, 15, 1e9, 10, 7123, 25)».Int -> $start, $count {
    put "\nFirst $count gapful numbers starting at {comma $start}:\n" ~
    <Sir Lord Duke King>.pick ~ ": ", ~
    ($start..*).grep( { $_ %% .comb[0, *-1].join } )[^$count];
}
```

#### Output:
```
First 30 gapful numbers starting at 100:
Sir: 100 105 108 110 120 121 130 132 135 140 143 150 154 160 165 170 176 180 187 190 192 195 198 200 220 225 231 240 242 253

First 15 gapful numbers starting at 1,000,000:
Duke: 1000000 1000005 1000008 1000010 1000016 1000020 1000021 1000030 1000032 1000034 1000035 1000040 1000050 1000060 1000065

First 10 gapful numbers starting at 1,000,000,000:
King: 1000000000 1000000001 1000000005 1000000008 1000000010 1000000016 1000000020 1000000027 1000000030 1000000032

First 25 gapful numbers starting at 7,123:
King: 7125 7140 7171 7189 7210 7272 7275 7280 7296 7350 7373 7420 7425 7474 7488 7490 7560 7575 7630 7632 7676 7700 7725 7770 7777
```
