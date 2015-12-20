[1]: http://rosettacode.org/wiki/Josephus_problem

# [Josephus problem][1]

Straightforward implementation of the executioner's algorithm:

```perl
sub Execute(@prisoner, $k) {
    until @prisoner == 1 {
	@prisoner.=rotate($k - 1);
	@prisoner.shift;
    }
}
 
my @prisoner = ^41;
Execute @prisoner, 3;
say "Prisoner {@prisoner} survived.";
```

#### Output:
```
Prisoner 30 survived.
```


We don't have to use numbers. Any list will do:

```perl
my @dalton = <Joe Jack William Averell Rantanplan>;
Execute @dalton, 2;
say "{@dalton} survived.";
```

#### Output:
```
William survived.
```