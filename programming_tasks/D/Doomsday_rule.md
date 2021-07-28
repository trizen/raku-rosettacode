[1]: https://rosettacode.org/wiki/Doomsday_rule

# [Doomsday rule][1]

```perl
my @dow = < Sunday Monday Tuesday Wednesday Thursday Friday Saturday >;
 
my %doomsday = False => [3,7,7,4,2,6,4,1,5,3,7,5], True => [4,1,7,4,2,6,4,1,5,3,7,5];
 
sub conway ($date) {
    my ($year, $month, $day) = $date.comb(/\d+/)».Int;
    my $is-leap = ($year %% 4) && (($year % 100) || ($year %% 400));
 
    my $c = $year div 100;
    my $s = ($year % 100) div 12;
    my $t = ($year % 100)   % 12;
    my $a = ( 5 * ($c % 4) + 2 ) % 7;
    my $b = ( $s + $t + ($t div 4) + $a ) % 7;
 
    ($b + $day - %doomsday{$is-leap}[$month - 1] + 7) % 7
}
 
 
for < 1800-01-06 1875-03-29 1915-12-07 1970-12-23 2043-05-14 2077-02-12 2101-04-02 >
{
    say "Conway  - $_ is a: ", @dow[.&conway];
    # Or, we could use the method built into the compiler...
    say "Builtin - $_ is a: ", @dow[Date.new($_).day-of-week];
    say '';
}
```

#### Output:
```
Conway  - 1800-01-06 is a: Monday
Builtin - 1800-01-06 is a: Monday

Conway  - 1875-03-29 is a: Monday
Builtin - 1875-03-29 is a: Monday

Conway  - 1915-12-07 is a: Tuesday
Builtin - 1915-12-07 is a: Tuesday

Conway  - 1970-12-23 is a: Wednesday
Builtin - 1970-12-23 is a: Wednesday

Conway  - 2043-05-14 is a: Thursday
Builtin - 2043-05-14 is a: Thursday

Conway  - 2077-02-12 is a: Friday
Builtin - 2077-02-12 is a: Friday

Conway  - 2101-04-02 is a: Saturday
Builtin - 2101-04-02 is a: Saturday
```
