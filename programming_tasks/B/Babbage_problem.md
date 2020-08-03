[1]: https://rosettacode.org/wiki/Babbage_problem

# [Babbage problem][1]

This could certainly be written more concisely. Extra verbiage is included to make the process more clear.

```raku
# For all positives integers from 1 to Infinity
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


Alternatively, the following just may be declarative enough to allow Babbage to understand what's going on:

```raku
say $_ if ($_²  % 1000000 == 269696) for 1..99736;
```

#### Output:
```
25264
99736
```