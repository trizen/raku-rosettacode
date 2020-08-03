[1]: https://rosettacode.org/wiki/Eban_numbers

# [Eban numbers][1]

Modular approach, very little is hard coded. Change the $upto order-of-magnitude limit to adjust the search/display ranges. Change the letter(s) given to the nban sub to modify which letter(s) to disallow.



Will handle multi-character 'bans'. Demonstrate for e-ban, t-ban and subur-ban.



Directly find&#160;:

```perl
use Lingua::EN::Numbers::Cardinal;

sub nban ($seq, $n = 'e') { ($seq).map: { next if .&cardinal.contains(any($n.comb)); $_ } }

sub filter ($n, $upto) {
    my @ban    = flat ((1 .. 99),).map: *.&nban($n);
    my @orders = (2 .. $upto).map({ 10**$_ X* 1..9 }).map: *.&nban($n);
    for @orders -> @order {
        next unless +@order;
        my @these;
        @these.append: flat $_, flat @ban X+ $_ for @order;
        @ban.append: @these
    }
    @ban.unshift(0) if 0.&nban($n);
    @ban
}

sub comma { $^i.flip.comb(3).join(',').flip }

for 'e', 't', 'subur' -> $n {
    my $upto = 10; # 1e10
    my @bans = filter($n, $upto);

    # DISPLAY
    my @k = @bans.grep: * < 1000;
    my @j = @bans.grep: 1000 <= * <= 4000;
    put "\n============= {$n}-ban: =============\n" ~
        "{$n}-ban numbers up to 1000: {+@k}\n{@k».&comma.gist}\n\n" ~
        "{$n}-ban numbers between 1,000 & 4,000: {+@j}\n{@j».&comma.gist}\n";

    for (1 .. $upto).map: { 10**$_ } -> $e {
        my $f = @bans.first( * > $e, :k );
        printf "Up to %14s: %s\n", comma($e), comma($f ?? +@bans[^$f] !! +@bans);
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

Up to             10: 3
Up to            100: 19
Up to          1,000: 19
Up to         10,000: 79
Up to        100,000: 399
Up to      1,000,000: 399
Up to     10,000,000: 1,599
Up to    100,000,000: 7,999
Up to  1,000,000,000: 7,999
Up to 10,000,000,000: 31,999

============= t-ban: =============
t-ban numbers up to 1000: 56
[0 1 4 5 6 7 9 11 100 101 104 105 106 107 109 111 400 401 404 405 406 407 409 411 500 501 504 505 506 507 509 511 600 601 604 605 606 607 609 611 700 701 704 705 706 707 709 711 900 901 904 905 906 907 909 911]

t-ban numbers between 1,000 & 4,000: 0
[]

Up to             10: 7
Up to            100: 9
Up to          1,000: 56
Up to         10,000: 56
Up to        100,000: 56
Up to      1,000,000: 57
Up to     10,000,000: 392
Up to    100,000,000: 393
Up to  1,000,000,000: 2,745
Up to 10,000,000,000: 19,208

============= subur-ban: =============
subur-ban numbers up to 1000: 35
[1 2 5 8 9 10 11 12 15 18 19 20 21 22 25 28 29 50 51 52 55 58 59 80 81 82 85 88 89 90 91 92 95 98 99]

subur-ban numbers between 1,000 & 4,000: 0
[]

Up to             10: 6
Up to            100: 35
Up to          1,000: 35
Up to         10,000: 35
Up to        100,000: 35
Up to      1,000,000: 36
Up to     10,000,000: 216
Up to    100,000,000: 1,295
Up to  1,000,000,000: 1,295
Up to 10,000,000,000: 1,295
```
