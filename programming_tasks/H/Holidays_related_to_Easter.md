[1]: https://rosettacode.org/wiki/Holidays_related_to_Easter

# [Holidays related to Easter][1]

```perl
my @abbr = < Nil Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec >;
 
my @holidays =
    Easter    => 0,
    Ascension => 39,
    Pentecost => 49,
    Trinity   => 56,
    Corpus    => 60;
 
sub easter($year) {
    my \ay = $year % 19;
    my \by = $year div 100;
    my \cy = $year % 100;
    my \dy = by div 4;
    my \ey = by % 4;
    my \fy = (by + 8) div 25;
    my \gy = (by - fy + 1) div 3;
    my \hy = (ay * 19 + by - dy - gy + 15) % 30;
    my \iy = cy div 4;
    my \ky = cy % 4;
    my \ly = (32 + 2 * ey + 2 * iy - hy - ky) % 7;
    my \md = hy + ly - 7 * ((ay + 11 * hy + 22 * ly) div 451) + 114;
    my \month = md div 31;
    my \day = md % 31 + 1;
 
    return month, day;
}
 
sub cholidays($year) {
    my ($emon, $eday) = easter($year);
    printf "%4s: ", $year;
    say join ', ', gather for @holidays -> $holiday {
	my $d = Date.new($year,$emon,$eday) + $holiday.value;
	take "{$holiday.key}: $d.day-of-month.fmt('%02s') @abbr[$d.month]";
    }
}
 
for flat (400,500 ... 2000), (2010 ... 2020), 2100 -> $year {
    cholidays($year);
}
```

#### Output:
```
 400: Easter: 02 Apr, Ascension: 11 May, Pentecost: 21 May, Trinity: 28 May, Corpus: 01 Jun
 500: Easter: 04 Apr, Ascension: 13 May, Pentecost: 23 May, Trinity: 30 May, Corpus: 03 Jun
 600: Easter: 13 Apr, Ascension: 22 May, Pentecost: 01 Jun, Trinity: 08 Jun, Corpus: 12 Jun
 700: Easter: 15 Apr, Ascension: 24 May, Pentecost: 03 Jun, Trinity: 10 Jun, Corpus: 14 Jun
 800: Easter: 23 Apr, Ascension: 01 Jun, Pentecost: 11 Jun, Trinity: 18 Jun, Corpus: 22 Jun
 900: Easter: 28 Mar, Ascension: 06 May, Pentecost: 16 May, Trinity: 23 May, Corpus: 27 May
1000: Easter: 30 Mar, Ascension: 08 May, Pentecost: 18 May, Trinity: 25 May, Corpus: 29 May
1100: Easter: 08 Apr, Ascension: 17 May, Pentecost: 27 May, Trinity: 03 Jun, Corpus: 07 Jun
1200: Easter: 09 Apr, Ascension: 18 May, Pentecost: 28 May, Trinity: 04 Jun, Corpus: 08 Jun
1300: Easter: 18 Apr, Ascension: 27 May, Pentecost: 06 Jun, Trinity: 13 Jun, Corpus: 17 Jun
1400: Easter: 20 Apr, Ascension: 29 May, Pentecost: 08 Jun, Trinity: 15 Jun, Corpus: 19 Jun
1500: Easter: 01 Apr, Ascension: 10 May, Pentecost: 20 May, Trinity: 27 May, Corpus: 31 May
1600: Easter: 02 Apr, Ascension: 11 May, Pentecost: 21 May, Trinity: 28 May, Corpus: 01 Jun
1700: Easter: 11 Apr, Ascension: 20 May, Pentecost: 30 May, Trinity: 06 Jun, Corpus: 10 Jun
1800: Easter: 13 Apr, Ascension: 22 May, Pentecost: 01 Jun, Trinity: 08 Jun, Corpus: 12 Jun
1900: Easter: 15 Apr, Ascension: 24 May, Pentecost: 03 Jun, Trinity: 10 Jun, Corpus: 14 Jun
2000: Easter: 23 Apr, Ascension: 01 Jun, Pentecost: 11 Jun, Trinity: 18 Jun, Corpus: 22 Jun
2010: Easter: 04 Apr, Ascension: 13 May, Pentecost: 23 May, Trinity: 30 May, Corpus: 03 Jun
2011: Easter: 24 Apr, Ascension: 02 Jun, Pentecost: 12 Jun, Trinity: 19 Jun, Corpus: 23 Jun
2012: Easter: 08 Apr, Ascension: 17 May, Pentecost: 27 May, Trinity: 03 Jun, Corpus: 07 Jun
2013: Easter: 31 Mar, Ascension: 09 May, Pentecost: 19 May, Trinity: 26 May, Corpus: 30 May
2014: Easter: 20 Apr, Ascension: 29 May, Pentecost: 08 Jun, Trinity: 15 Jun, Corpus: 19 Jun
2015: Easter: 05 Apr, Ascension: 14 May, Pentecost: 24 May, Trinity: 31 May, Corpus: 04 Jun
2016: Easter: 27 Mar, Ascension: 05 May, Pentecost: 15 May, Trinity: 22 May, Corpus: 26 May
2017: Easter: 16 Apr, Ascension: 25 May, Pentecost: 04 Jun, Trinity: 11 Jun, Corpus: 15 Jun
2018: Easter: 01 Apr, Ascension: 10 May, Pentecost: 20 May, Trinity: 27 May, Corpus: 31 May
2019: Easter: 21 Apr, Ascension: 30 May, Pentecost: 09 Jun, Trinity: 16 Jun, Corpus: 20 Jun
2020: Easter: 12 Apr, Ascension: 21 May, Pentecost: 31 May, Trinity: 07 Jun, Corpus: 11 Jun
2100: Easter: 28 Mar, Ascension: 06 May, Pentecost: 16 May, Trinity: 23 May, Corpus: 27 May
```