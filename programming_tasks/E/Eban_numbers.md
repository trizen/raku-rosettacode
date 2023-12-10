[1]: https://rosettacode.org/wiki/Eban_numbers

# [Eban numbers][1]





Modular approach, very little is hard coded. Change the $upto order-of-magnitude limit to adjust the search/display ranges. Change the letter(s) given to the enumerate / count subs to modify which letter(s) to disallow.



Will handle multi-character 'bans'. Demonstrate for e-ban, t-ban and subur-ban.



Directly find&#160;:



Considering numbers up to <strong>10<sup>21</sup></strong>, as the task directions suggest.

```perl
use Lingua::EN::Numbers;

sub nban ($seq, $n = 'e') { ($seq).map: { next if .&cardinal.contains(any($n.lc.comb)); $_ } }

sub enumerate ($n, $upto) {
    my @ban = [nban(1 .. 99, $n)],;
    my @orders;
    (2 .. $upto).map: -> $o {
        given $o % 3 { # Compensate for irregulars: 11 - 19
            when 1  { @orders.push: [flat (10**($o - 1) X* 10 .. 19).map(*.&nban($n)), |(10**$o X* 2 .. 9).map: *.&nban($n)] }
            default { @orders.push: [flat (10**$o X* 1 .. 9).map: *.&nban($n)] }
        }
    }
    ^@orders .map: -> $o {
        @ban.push: [] and next unless +@orders[$o];
        my @these;
        @orders[$o].map: -> $m {
            @these.push: $m;
            for ^@ban -> $b {
                next unless +@ban[$b];
                @these.push: $_ for (flat @ban[$b]) »+» $m ;
            }
        }
        @ban.push: @these;
    }
    @ban.unshift(0) if nban(0, $n);
    flat @ban.map: *.flat;
}

sub count ($n, $upto) {
    my @orders;
    (2 .. $upto).map: -> $o {
        given $o % 3 { # Compensate for irregulars: 11 - 19
            when 1  { @orders.push: [flat (10**($o - 1) X* 10 .. 19).map(*.&nban($n)), |(10**$o X* 2 .. 9).map: *.&nban($n)] }
            default { @orders.push: [flat (10**$o X* 1 .. 9).map: *.&nban($n)] }
        }
    }
    my @count  = +nban(1 .. 99, $n);
    ^@orders .map: -> $o {
        @count.push: 0 and next unless +@orders[$o];
        my $prev = so (@orders[$o].first( { $_ ~~ /^ '1' '0'+ $/ } ) // 0 );
        my $sum = @count.sum;
        my $these = +@orders[$o] * $sum + @orders[$o];
        $these-- if $prev;
        @count[1 + $o] += $these;
        ++@count[$o]  if $prev;
    }
    ++@count[0] if nban(0, $n);
    [\+] @count;
}

#for < e o t tali subur tur ur cali i u > -> $n { # All of them
for < e t subur > -> $n { # An assortment for demonstration
    my $upto   = 21; # 1e21
    my @bans   = enumerate($n, 4);
    my @counts = count($n, $upto);

    # DISPLAY
    my @k = @bans.grep: * < 1000;
    my @j = @bans.grep: 1000 <= * <= 4000;
    put "\n============= {$n}-ban: =============\n" ~
        "{$n}-ban numbers up to 1000: {+@k}\n[{@k».&comma}]\n\n" ~
        "{$n}-ban numbers between 1,000 & 4,000: {+@j}\n[{@j».&comma}]\n" ~
        "\nCounts of {$n}-ban numbers up to {cardinal 10**$upto}"
        ;

    my $s = max (1..$upto).map: { (10**$_).&cardinal.chars };
    @counts.unshift: @bans.first: * > 10, :k;
    for ^$upto -> $c {
        printf "Up to and including %{$s}s: %s\n", cardinal(10**($c+1)), comma(@counts[$c]);
    }
}
```

#### Output:
```
============= e-ban: =============
e-ban numbers up to 1000: 19
[2 4 6 30 32 34 36 40 42 44 46 50 52 54 56 60 62 64 66]

e-ban numbers between 1,000 & 4,000: 21
[2,000 2,002 2,004 2,006 2,030 2,032 2,034 2,036 2,040 2,042 2,044 2,046 2,050 2,052 2,054 2,056 2,060 2,062 2,064 2,066 4,000]

Counts of e-ban numbers up to one sextillion
Up to and including                     ten: 3
Up to and including             one hundred: 19
Up to and including            one thousand: 19
Up to and including            ten thousand: 79
Up to and including    one hundred thousand: 399
Up to and including             one million: 399
Up to and including             ten million: 1,599
Up to and including     one hundred million: 7,999
Up to and including             one billion: 7,999
Up to and including             ten billion: 31,999
Up to and including     one hundred billion: 159,999
Up to and including            one trillion: 159,999
Up to and including            ten trillion: 639,999
Up to and including    one hundred trillion: 3,199,999
Up to and including         one quadrillion: 3,199,999
Up to and including         ten quadrillion: 12,799,999
Up to and including one hundred quadrillion: 63,999,999
Up to and including         one quintillion: 63,999,999
Up to and including         ten quintillion: 255,999,999
Up to and including one hundred quintillion: 1,279,999,999
Up to and including          one sextillion: 1,279,999,999

============= t-ban: =============
t-ban numbers up to 1000: 56
[0 1 4 5 6 7 9 11 100 101 104 105 106 107 109 111 400 401 404 405 406 407 409 411 500 501 504 505 506 507 509 511 600 601 604 605 606 607 609 611 700 701 704 705 706 707 709 711 900 901 904 905 906 907 909 911]

t-ban numbers between 1,000 & 4,000: 0
[]

Counts of t-ban numbers up to one sextillion
Up to and including                     ten: 7
Up to and including             one hundred: 9
Up to and including            one thousand: 56
Up to and including            ten thousand: 56
Up to and including    one hundred thousand: 56
Up to and including             one million: 57
Up to and including             ten million: 392
Up to and including     one hundred million: 785
Up to and including             one billion: 5,489
Up to and including             ten billion: 38,416
Up to and including     one hundred billion: 76,833
Up to and including            one trillion: 537,824
Up to and including            ten trillion: 537,824
Up to and including    one hundred trillion: 537,824
Up to and including         one quadrillion: 537,825
Up to and including         ten quadrillion: 3,764,768
Up to and including one hundred quadrillion: 7,529,537
Up to and including         one quintillion: 52,706,752
Up to and including         ten quintillion: 52,706,752
Up to and including one hundred quintillion: 52,706,752
Up to and including          one sextillion: 52,706,752

============= subur-ban: =============
subur-ban numbers up to 1000: 35
[1 2 5 8 9 10 11 12 15 18 19 20 21 22 25 28 29 50 51 52 55 58 59 80 81 82 85 88 89 90 91 92 95 98 99]

subur-ban numbers between 1,000 & 4,000: 0
[]

Counts of subur-ban numbers up to one sextillion
Up to and including                     ten: 6
Up to and including             one hundred: 35
Up to and including            one thousand: 35
Up to and including            ten thousand: 35
Up to and including    one hundred thousand: 35
Up to and including             one million: 36
Up to and including             ten million: 216
Up to and including     one hundred million: 2,375
Up to and including             one billion: 2,375
Up to and including             ten billion: 2,375
Up to and including     one hundred billion: 2,375
Up to and including            one trillion: 2,375
Up to and including            ten trillion: 2,375
Up to and including    one hundred trillion: 2,375
Up to and including         one quadrillion: 2,375
Up to and including         ten quadrillion: 2,375
Up to and including one hundred quadrillion: 2,375
Up to and including         one quintillion: 2,375
Up to and including         ten quintillion: 2,375
Up to and including one hundred quintillion: 2,375
Up to and including          one sextillion: 2,375
```


Note that the limit to one sextillion is somewhat arbitrary and is just to match the task parameters.



This will quite happily count \*-bans up to one hundred centillion. (10<sup>305</sup>) It takes longer, but still on the order of seconds, not minutes.


```
Counts of e-ban numbers up to one hundred centillion
 ...
Up to and including one hundred centillion: 35,184,372,088,831,999,999,999,999,999,999,999,999,999,999,999,999,999,999,999
```
