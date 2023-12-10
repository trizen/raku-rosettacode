[1]: https://rosettacode.org/wiki/Babbage_problem

# [Babbage problem][1]





This could certainly be written more concisely. Extra verbiage is included to make the process more clear.

```perl
# For all positive integers from 1 to Infinity
for 1 .. Inf -> $integer {
    
    # calculate the square of the integer
    my $square = $integer²;
    
    # print the integer and square and exit if the square modulo 1000000 is equal to 269696
    print "{$integer}² equals $square" and exit if $square mod 1000000 == 269696;
}
```

#### Output:
```
25264² equals 638269696
```


More concisely (but clear enough to allow Mr. Babbage to understand what's going on?):

```perl
.say and exit if $_²  % 1e6 == 269696 for ^∞;
```

#### Output:
```
25264
```


Notice that \`exit\` ends the process itself instead of ending just the loop, as \`last\` would.  \`exit\` would arguably be easier to understand for Babbage though, so it may better fill the task requirement.
