[1]: http://rosettacode.org/wiki/Pathological_floating_point_problems

# [Pathological floating point problems][1]

The simple solution to doing calculations where floating point numbers exhibit pathological behavior is: don't do floating point calculations.&#160;:-) Perl 6 is just as susceptible to floating point error as any other C based language, however, it offers built-in rational Types; where numbers are represented as a ratio of two integers. For normal precision it uses Rats - accurate to 1/2^64, and for arbitrary precision, FatRats, which can grow as large as available memory. Rats don't require any special special setup to use. Any decimal number within its limits of precision is automatically stored as a Rat. FatRats require explicit coercion and are "sticky". Any FatRat operand in a calculation will cause all further results to be stored as FatRats.

```perl
say '1st: Convergent series';
my @series = 2.FatRat, -4, { 111 - 1130 / $^v + 3000 / ( $^v * $^u ) } ... *;
for flat 3..8, 20, 30, 50, 100 -> $n {say "n = {$n.fmt("%3d")} @series[$n-1]"};
 
say "\n2nd: Chaotic banking society";
sub postfix:<!> (Int $n) { [*] 2..$n } # factorial operator
my $years = 25;
my $balance = sum map { 1 / FatRat.new($_!) }, 1 .. $years + 15; # Generate e-1  to sufficient precision with a Taylor series
put "Starting balance, \$(e-1): \$$balance";
for 1..$years -> $i { $balance = $i * $balance - 1 }
printf("After year %d, you will have \$%1.16g in your account.\n", $years, $balance);
 
print "\n3rd: Rump's example: f(77617.0, 33096.0) = ";
sub f (\a, \b) { 333.75*b⁶ + a²*( 11*a²*b² - b⁶ - 121*b⁴ - 2) + 5.5*b⁸ + a/(2*b) }
say f(77617.0, 33096.0).fmt("%0.16g");
```

#### Output:
```
1st: Convergent series
n =   3 18.5
n =   4 9.378378
n =   5 7.801153
n =   6 7.154414
n =   7 6.806785
n =   8 6.5926328
n =  20 6.0435521101892689
n =  30 6.006786093031205758530554
n =  50 6.0001758466271871889456140207471954695237
n = 100 6.000000019319477929104086803403585715024350675436952458072592750856521767230266

2nd: Chaotic banking society
Starting balance, $(e-1): $1.7182818284590452353602874713526624977572470936999
After year 25, you will have $0.0399387296732302 in your account.

3rd: Rump's example: f(77617.0, 33096.0) = -0.827396059946821
```