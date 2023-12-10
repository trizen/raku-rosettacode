[1]: https://rosettacode.org/wiki/N-smooth_numbers

# [N-smooth numbers][1]



```perl
sub smooth-numbers (*@list) {
    cache my \Smooth := gather {
        my %i = (flat @list) Z=> (Smooth.iterator for ^@list);
        my %n = (flat @list) Z=> 1 xx *;
 
        loop {
            take my $n := %n{*}.min;
 
            for @list -> \k {
                %n{k} = %i{k}.pull-one * k if %n{k} == $n;
            }
        }
    }
}

sub abbrev ($n) {
   $n.chars > 50 ??
   $n.substr(0,10) ~ "...({$n.chars - 20} digits omitted)..." ~ $n.substr(* - 10) !!
   $n
}
 
my @primes = (2..*).grep: *.is-prime;
 
my $start = 3000;
 
for ^@primes.first( * > 29, :k ) -> $p {
    put join "\n", "\nFirst 25, and {$start}th through {$start+2}nd {@primes[$p]}-smooth numbers:",
    $(smooth-numbers(|@primes[0..$p])[^25]),
    $(smooth-numbers(|@primes[0..$p])[$start - 1 .. $start + 1]».&abbrev);
}
 
$start = 30000;
 
for 503, 509, 521 -> $p {
    my $i = @primes.first( * == $p, :k );
    put "\n{$start}th through {$start+19}th {@primes[$i]}-smooth numbers:\n" ~
    smooth-numbers(|@primes[0..$i])[$start - 1 .. $start + 18];
}
```

#### Output:
```
First 25, and 3000th through 3002nd 2-smooth numbers:
1 2 4 8 16 32 64 128 256 512 1024 2048 4096 8192 16384 32768 65536 131072 262144 524288 1048576 2097152 4194304 8388608 16777216
6151159610...(883 digits omitted)...9114994688 1230231922...(884 digits omitted)...8229989376 2460463844...(884 digits omitted)...6459978752

First 25, and 3000th through 3002nd 3-smooth numbers:
1 2 3 4 6 8 9 12 16 18 24 27 32 36 48 54 64 72 81 96 108 128 144 162 192
91580367978306252441724649472 92829823186414819915547541504 94096325042746502515294076928

First 25, and 3000th through 3002nd 5-smooth numbers:
1 2 3 4 5 6 8 9 10 12 15 16 18 20 24 25 27 30 32 36 40 45 48 50 54
278942752080 279936000000 281250000000

First 25, and 3000th through 3002nd 7-smooth numbers:
1 2 3 4 5 6 7 8 9 10 12 14 15 16 18 20 21 24 25 27 28 30 32 35 36
50176000 50331648 50388480

First 25, and 3000th through 3002nd 11-smooth numbers:
1 2 3 4 5 6 7 8 9 10 11 12 14 15 16 18 20 21 22 24 25 27 28 30 32
2112880 2116800 2117016

First 25, and 3000th through 3002nd 13-smooth numbers:
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 18 20 21 22 24 25 26 27 28
390000 390390 390625

First 25, and 3000th through 3002nd 17-smooth numbers:
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 20 21 22 24 25 26 27
145800 145860 146016

First 25, and 3000th through 3002nd 19-smooth numbers:
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 24 25 26
74256 74358 74360

First 25, and 3000th through 3002nd 23-smooth numbers:
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
46552 46575 46585

First 25, and 3000th through 3002nd 29-smooth numbers:
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
33516 33524 33534

30000th through 30019th 503-smooth numbers:
62913 62914 62916 62918 62920 62923 62926 62928 62930 62933 62935 62937 62944 62946 62951 62952 62953 62957 62959 62964

30000th through 30019th 509-smooth numbers:
62601 62602 62604 62607 62608 62609 62611 62618 62620 62622 62624 62625 62626 62628 62629 62634 62640 62643 62645 62646

30000th through 30019th 521-smooth numbers:
62287 62288 62291 62292 62300 62304 62307 62308 62310 62315 62320 62321 62322 62325 62328 62329 62330 62331 62335 62336
```
