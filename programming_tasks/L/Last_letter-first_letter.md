[1]: http://rosettacode.org/wiki/Last_letter-first_letter

# [Last letter-first letter][1]

A breadth-first search that uses disk files to avoid memory exhaustion. Each candidate sequence is encoded at one character per name, so to avoid reuse of names we merely have to make sure there are no repeat characters in our encoded string. (The encoding starts at ASCII space for the first name, so newline is not among the encoded characters.)

```perl
my @names = <
    audino bagon baltoy banette bidoof braviary bronzor carracosta charmeleon
    cresselia croagunk darmanitan deino emboar emolga exeggcute gabite
    girafarig gulpin haxorus heatmor heatran ivysaur jellicent jumpluff kangaskhan
    kricketune landorus ledyba loudred lumineon lunatone machamp magnezone mamoswine
    nosepass petilil pidgeotto pikachu pinsir poliwrath poochyena porygon2
    porygonz registeel relicanth remoraid rufflet sableye scolipede scrafty seaking
    sealeo silcoon simisear snivy snorlax spoink starly tirtouga trapinch treecko
    tyrogue vigoroth vulpix wailord wartortle whismur wingull yamask
>;
 
my @last = @names.map: {.substr(*-1,1).ord }
my @succs = [] xx 128;
for @names.kv -> $i, $name {
    my $ix = $name.ord; # $name.substr(0,1).ord
    push @succs[$ix], $i;
}
 
my $OUT = open "llfl.new", :w or die "Can't create llfl.new: $!";
$OUT.print: chr($_ + 32),"\n" for 0 ..^ @names;
close $OUT;
my $new = +@names;
my $len = 1;
 
while $new {
    say "Length { $len++ }: $new candidates";
    shell 'mv llfl.new llfl.known';
    my $IN = open "llfl.known" or die "Can't reopen llfl.known: $!";
    my $OUT = open "llfl.new", :w or die "Can't create llfl.new: $!";
    $new = 0;
 
    loop {
	my $cand = $IN.get // last;
	for @succs[@last[$cand.ord - 32]][] -> $i {
	    my $ic = chr($i + 32);
	    next if $cand ~~ /$ic/;
	    $OUT.print: $ic,$cand,"\n";
	    $new++;
	}
    }
    $IN.close;
    $OUT.close;
}
 
my $IN = open "llfl.known" or die "Can't reopen llfl.known: $!";
my $eg = $IN.lines.pick;
say "Length of longest: ", $eg.chars;
say join ' ', $eg.ords.reverse.map: { @names[$_ - 32] }
```

#### Output:
```
Length 1: 70 candidates
Length 2: 172 candidates
Length 3: 494 candidates
Length 4: 1288 candidates
Length 5: 3235 candidates
Length 6: 7731 candidates
Length 7: 17628 candidates
Length 8: 37629 candidates
Length 9: 75122 candidates
Length 10: 139091 candidates
Length 11: 236679 candidates
Length 12: 367405 candidates
Length 13: 516210 candidates
Length 14: 650916 candidates
Length 15: 733915 candidates
Length 16: 727566 candidates
Length 17: 621835 candidates
Length 18: 446666 candidates
Length 19: 260862 candidates
Length 20: 119908 candidates
Length 21: 40296 candidates
Length 22: 10112 candidates
Length 23: 1248 candidates
Length of longest: 23
machamp petilil loudred darmanitan nosepass simisear rufflet trapinch heatmor registeel landorus starly yamask kricketune exeggcute emboar relicanth haxorus seaking girafarig gabite emolga audino
```