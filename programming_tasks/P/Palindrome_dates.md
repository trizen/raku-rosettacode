[1]: https://rosettacode.org/wiki/Palindrome_dates

# [Palindrome dates][1]

Pretty basic, but good enough. Could start earlier but 3/2/1 digit years require different handling that isn't necessary for this task. (And would be pretty pointless anyway assuming we need 2 digits for the month and two digits for the day. ISO:8601 anybody?)

```perl
my $start = '1000-01-01';
 
my @palindate = {
     state $year = $start.substr(0,4);
     ++$year;
     my $m = $year.substr(2, 2).flip;
     my $d = $year.substr(0, 2).flip;
     next if not try Date.new("$year-$m-$d");
     "$year-$m-$d"
} … *;
 
my $date-today = Date.today; # 2020-02-02
 
my $k = @palindate.first: { Date.new($_) > $date-today }, :k;
 
say join "\n", @palindate[$k - 1 .. $k + 14];
 
say "\nTotal number of four digit year palindrome dates:\n" ~
my $four = @palindate.first( { .substr(5,1) eq '-' }, :k );
say "between {@palindate[0]} and {@palindate[$four - 1]}.";
 
my $five = @palindate.first: { .substr(6,1) eq '-' }, :k;
 
say "\nTotal number of five digit year palindrome dates:\n" ~
+@palindate[$four .. $five]
```

#### Output:
```
2020-02-02
2021-12-02
2030-03-02
2040-04-02
2050-05-02
2060-06-02
2070-07-02
2080-08-02
2090-09-02
2101-10-12
2110-01-12
2111-11-12
2120-02-12
2121-12-12
2130-03-12
2140-04-12

Total number of four digit year palindrome dates:
331
between 1001-10-01 and 9290-09-29.

Total number of five digit year palindrome dates:
3303
```
