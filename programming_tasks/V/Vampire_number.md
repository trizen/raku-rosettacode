[1]: https://rosettacode.org/wiki/Vampire_number

# [Vampire number][1]



```perl
sub is_vampire (Int $num) {
    my $digits = $num.comb.sort;
    my @fangs;
    (10**$num.sqrt.log(10).floor .. $num.sqrt.ceiling).map: -> $this {
        next if $num % $this;
        my $that = $num div $this;
        next if $this %% 10 && $that %% 10;
        @fangs.push("$this x $that") if ($this ~ $that).comb.sort eq $digits;
    }
    @fangs
}

constant @vampires = flat (3..*).map: -> $s, $e {
    (10**$s .. 10**$e).hyper.map: -> $n {
        next unless my @fangs = is_vampire($n);
        "$n: { @fangs.join(', ') }"
    }
}

say "\nFirst 25 Vampire Numbers:\n";

.say for @vampires[^25];

say "\nIndividual tests:\n";

.say for (16758243290880, 24959017348650, 14593825548650).hyper(:1batch).map: {
    "$_: " ~ (is_vampire($_).join(', ') || 'is not a vampire number.')
}
```

#### Output:
```
First 25 Vampire Numbers:

1260: 21 x 60
1395: 15 x 93
1435: 35 x 41
1530: 30 x 51
1827: 21 x 87
2187: 27 x 81
6880: 80 x 86
102510: 201 x 510
104260: 260 x 401
105210: 210 x 501
105264: 204 x 516
105750: 150 x 705
108135: 135 x 801
110758: 158 x 701
115672: 152 x 761
116725: 161 x 725
117067: 167 x 701
118440: 141 x 840
120600: 201 x 600
123354: 231 x 534
124483: 281 x 443
125248: 152 x 824
125433: 231 x 543
125460: 204 x 615, 246 x 510
125500: 251 x 500

Individual tests:

16758243290880: 1982736 x 8452080, 2123856 x 7890480, 2751840 x 6089832, 2817360 x 5948208
24959017348650: 2947050 x 8469153, 2949705 x 8461530, 4125870 x 6049395, 4129587 x 6043950, 4230765 x 5899410
14593825548650: is not a vampire number.
```
