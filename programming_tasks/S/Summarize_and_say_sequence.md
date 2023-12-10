[1]: https://rosettacode.org/wiki/Summarize_and_say_sequence

# [Summarize and say sequence][1]



```perl
my @list;
my $longest = 0;
my %seen;

for 1 .. 1000000 -> $m {
    next unless $m ~~ /0/;         # seed must have a zero
    my $j = join '', $m.comb.sort;
    next if %seen{$j}:exists;      # already tested a permutation
    %seen{$j} = '';
    my @seq = converging($m);
    my %elems;
    my $count;
    for @seq[] -> $value { last if ++%elems{$value} == 2; $count++; };
    if $longest == $count {
        @list.push($m);
    }
    elsif $longest < $count {
        $longest = $count;
        @list = $m;
        print "\b" x 20, "$count, $m"; # monitor progress
    }
};

for @list -> $m {
    say "\nSeed Value(s): ", my $seeds = ~permutations($m).unique.grep( { .substr(0,1) != 0 } );
    my @seq = converging($m);
    my %elems;
    my $count;
    for @seq[] -> $value { last if ++%elems{$value} == 2; $count++; };
    say "\nIterations: ", $count;
    say "\nSequence: (Only one shown per permutation group.)";
   .say for |@seq[^$count], "\n";
}

sub converging ($seed) { return $seed, -> $l { join '', map { $_.value.elems~$_.key }, $l.comb.classify({$^b}).sort: {-$^c.key} } ... * }

sub permutations ($string, $sofar? = '' ) {
    return $sofar unless $string.chars;
    my @perms;
    for ^$string.chars -> $idx {
        my $this = $string.substr(0,$idx)~$string.substr($idx+1);
        my $char = substr($string, $idx,1);
        @perms.push( |permutations( $this, join '', $sofar, $char ) );
    }
    return @perms;
}
```

#### Output:
```
Seed Value(s): 9009 9090 9900

Iterations: 21

Sequence: (Only one shown per permutation group.)
9009
2920
192210
19222110
19323110
1923123110
1923224110
191413323110
191433125110
19151423125110
19251413226110
1916151413325110
1916251423127110
191716151413326110
191726151423128110
19181716151413327110
19182716151423129110
29181716151413328110
19281716151423228110
19281716151413427110
19182716152413228110
```
