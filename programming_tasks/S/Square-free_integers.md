[1]: https://rosettacode.org/wiki/Square-free_integers

# [Square-free integers][1]

The prime factoring algorithm is not really the best option for finding long runs of sequential square-free numbers. It works, but is probably better suited for testing arbitrary numbers rather than testing every sequential number from 1 to some limit. If you know that that is going to be your use case, there are faster algorithms.

```raku
# Prime factorization routines
sub prime-factors ( Int $n where * > 0 ) {
    return $n if $n.is-prime;
    return [] if $n == 1;
    my $factor = find-factor( $n );
    flat prime-factors( $factor ), prime-factors( $n div $factor );
}
 
sub find-factor ( Int $n, $constant = 1 ) {
    return 2 unless $n +& 1;
    if (my $gcd = $n gcd 6541380665835015) > 1 {
        return $gcd if $gcd != $n
    }
    my $x      = 2;
    my $rho    = 1;
    my $factor = 1;
    while $factor == 1 {
        $rho *= 2;
        my $fixed = $x;
        for ^$rho {
            $x = ( $x * $x + $constant ) % $n;
            $factor = ( $x - $fixed ) gcd $n;
            last if 1 < $factor;
        }
    }
    $factor = find-factor( $n, $constant + 1 ) if $n == $factor;
    $factor;
}
 
# Task routine
sub is-square-free (Int $n) { my @v = $n.&prime-factors.Bag.values; @v.sum/@v <= 1 }
 
# The Task
# Parts 1 & 2
for 1, 145, 1e12.Int, 145+1e12.Int -> $start, $end {
    say "\nSquare─free numbers between $start and $end:\n",
    ($start .. $end).hyper(:4batch).grep( *.&is-square-free ).list.fmt("%3d").comb(84).join("\n");
}
 
# Part 3
for 1e2, 1e3, 1e4, 1e5, 1e6 {
    say "\nThe number of square─free numbers between 1 and {$_} (inclusive) is: ",
    +(1 .. .Int).race.grep: *.&is-square-free;
}
```

#### Output:
```
Square─free numbers between 1 and 145:
  1   2   3   5   6   7  10  11  13  14  15  17  19  21  22  23  26  29  30  31  33 
 34  35  37  38  39  41  42  43  46  47  51  53  55  57  58  59  61  62  65  66  67 
 69  70  71  73  74  77  78  79  82  83  85  86  87  89  91  93  94  95  97 101 102 
103 105 106 107 109 110 111 113 114 115 118 119 122 123 127 129 130 131 133 134 137 
138 139 141 142 143 145

Square─free numbers between 1000000000000 and 1000000000145:
1000000000001 1000000000002 1000000000003 1000000000005 1000000000006 1000000000007 
1000000000009 1000000000011 1000000000013 1000000000014 1000000000015 1000000000018 
1000000000019 1000000000021 1000000000022 1000000000023 1000000000027 1000000000029 
1000000000030 1000000000031 1000000000033 1000000000037 1000000000038 1000000000039 
1000000000041 1000000000042 1000000000043 1000000000045 1000000000046 1000000000047 
1000000000049 1000000000051 1000000000054 1000000000055 1000000000057 1000000000058 
1000000000059 1000000000061 1000000000063 1000000000065 1000000000066 1000000000067 
1000000000069 1000000000070 1000000000073 1000000000074 1000000000077 1000000000078 
1000000000079 1000000000081 1000000000082 1000000000085 1000000000086 1000000000087 
1000000000090 1000000000091 1000000000093 1000000000094 1000000000095 1000000000097 
1000000000099 1000000000101 1000000000102 1000000000103 1000000000105 1000000000106 
1000000000109 1000000000111 1000000000113 1000000000114 1000000000115 1000000000117 
1000000000118 1000000000119 1000000000121 1000000000122 1000000000123 1000000000126 
1000000000127 1000000000129 1000000000130 1000000000133 1000000000135 1000000000137 
1000000000138 1000000000139 1000000000141 1000000000142 1000000000145

The number of square─free numbers between 1 and 100 (inclusive) is: 61

The number of square─free numbers between 1 and 1000 (inclusive) is: 608

The number of square─free numbers between 1 and 10000 (inclusive) is: 6083

The number of square─free numbers between 1 and 100000 (inclusive) is: 60794

The number of square─free numbers between 1 and 1000000 (inclusive) is: 607926
```