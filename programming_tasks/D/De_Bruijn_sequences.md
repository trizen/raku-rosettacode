[1]: https://rosettacode.org/wiki/De_Bruijn_sequences

# [De Bruijn sequences][1]





Deviates very slightly from the task spec. Generates a randomized de Bruijn sequence and replaces the 4444th digit with a the digit plus 1 mod 10 rather than a '.', mostly so it can demonstrate detection of extra PINs as well as missing ones.

```perl
# Generate the sequence
my $seq;

for ^100 {
    my $a = .fmt: '%02d';
    next if $a.substr(1,1) < $a.substr(0,1);
    $seq ~= ($a.substr(0,1) == $a.substr(1,1)) ?? $a.substr(0,1) !! $a;
    for +$a ^..^ 100 {
        next if .fmt('%02d').substr(1,1) <= $a.substr(0,1);
        $seq ~= sprintf "%s%02d", $a, $_ ;
    }
}

$seq = $seq.comb.list.rotate((^10000).pick).join;

$seq ~= $seq.substr(0,3);

sub check ($seq) {
    my %chk;
    for ^($seq.chars) { %chk{$seq.substr( $_, 4 )}++ }
    put 'Missing: ', (^9999).grep( { not %chk{ .fmt: '%04d' } } ).fmt: '%04d';
    put 'Extra:   ', %chk.grep( *.value > 1 )».key.sort.fmt: '%04d';
}

## The Task
put "de Bruijn sequence length: " ~ $seq.chars;

put "\nFirst 130 characters:\n" ~ $seq.substr( 0, 130 );

put "\nLast 130 characters:\n" ~ $seq.substr( * - 130 );

put "\nIncorrect 4 digit PINs in this sequence:";
check $seq;

put "\nIncorrect 4 digit PINs in the reversed sequence:";
check $seq.flip;

my $digit = $seq.substr(4443,1);
put "\nReplacing the 4444th digit, ($digit) with { ($digit += 1) %= 10 }";
put "Incorrect 4 digit PINs in the revised sequence:";
$seq.substr-rw(4443,1) = $digit;
check $seq;
```

#### Output:
```
de Bruijn sequence length: 10003

First 130 characters:
4558455945654566456745684569457545764577457845794585458645874588458945954596459745984599464647464846494655465646574658465946654666

Last 130 characters:
5445644574458445944654466446744684469447544764477447844794485448644874488448944954496449744984499454546454745484549455545564557455

Incorrect 4 digit PINs in this sequence:
Missing: 
Extra:   

Incorrect 4 digit PINs in the reversed sequence:
Missing: 
Extra:   

Replacing the 4444th digit, (1) with 2
Incorrect 4 digit PINs in the revised sequence:
Missing: 0961 1096 6109 9610
Extra:   0962 2096 6209 9620
```
