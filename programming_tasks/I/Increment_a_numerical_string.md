[1]: https://rosettacode.org/wiki/Increment_a_numerical_string

# [Increment a numerical string][1]



```perl
say my $s = "12345";
say ++$s;

# or Unicode. How about Sinhala?

say "෧෨෩෪෫ ", +"෧෨෩෪෫";
say "෧෨෩෪෫".succ, ' ', +"෧෨෩෪෫".succ;
```

#### Output:
```
12345
12346
෧෨෩෪෫ 12345
෧෨෩෪෬ 12346
```
