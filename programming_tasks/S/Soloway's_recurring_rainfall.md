[1]: https://rosettacode.org/wiki/Soloway%27s_recurring_rainfall

# [Soloway&#039;s recurring rainfall][1]

This task seems to be more about following (kind of vague) instructions and anticipating error conditions rather than solving a rather simple problem.



I notice that units are specified for neither measurement; time nor rainfall accumulation. Time is assumed to always increment in units of one, rainfall in some non-negative integer. "Integer" gets a little leeway in this entry. Floating point numbers or Rational numbers that are exactly equal to an Integer are allowed, even if they are technically not integers.



#### After update to spec:



Not going to bother to allow negative rainfall amounts. The "Negative rainfall" cited in the linked article references "total accumulated groundwater" **NOT** precipitation. Evaporation isn't rainfall. Much like dew or fog is not rainfall.



**Further**, the linked reference article for Soloway's Recurring Rainfall <big>***SPECIFICALLY***</big> discusses marking an implementation as incorrect, or at least lacking, if it doesn't ignore and discard negative entries. You can't have it both ways.

```perl
# Write a program that will read in integers and
# output their average. Stop reading when the
# value 99999 is input.


my ($periods, $accumulation, $rainfall) = 0, 0;

loop {
    loop {
        $rainfall = prompt 'Integer units of rainfall in this time period? (999999 to finalize and exit)>: ';
        last if $rainfall.chars and $rainfall.Numeric !~~ Failure and $rainfall.narrow ~~ Int and $rainfall ≥ 0;
        say 'Invalid input, try again.';
    }
    last if $rainfall == 999999;
    ++$periods;
    $accumulation += $rainfall;
    say-it;
}

say-it;

sub say-it { printf "Average rainfall %.2f units over %d time periods.\n", ($accumulation / $periods) || 0, $periods }
```

#### Output:
```
# Normal operation
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 0
Average rainfall 0.00 units over 1 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 2
Average rainfall 1.00 units over 2 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 4
Average rainfall 2.00 units over 3 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 6
Average rainfall 3.00 units over 4 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 8
Average rainfall 4.00 units over 5 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 999999
Average rainfall 4.00 units over 5 time periods.


# Invalid entries and valid but not traditional "Integer" entries demonstrated  
Integer units of rainfall in this time period? (999999 to finalize and exit)>: a
Invalid input, try again.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 1.1
Invalid input, try again.
Integer units of rainfall in this time period? (999999 to finalize and exit)>:
Invalid input, try again.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 2.0
Average rainfall 2.00 units over 1 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: .3e1
Average rainfall 2.50 units over 2 time periods.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 999999
Average rainfall 2.50 units over 2 time periods.


# Valid summary with no entries demonstrated.
Integer units of rainfall in this time period? (999999 to finalize and exit)>: 999999
Average rainfall 0.00 units over 0 time periods.
```
