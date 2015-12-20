[1]: http://rosettacode.org/wiki/Find_largest_left_truncatable_prime_in_a_given_base

# [Find largest left truncatable prime in a given base][1]

Using the Miller-Rabin definition of <tt>is-prime</tt> from [Miller-Rabin primality test#Perl 6](/wiki/Miller-Rabin\_primality\_test#Perl\_6" title="Miller-Rabin primality test" class="mw-redirect), or the built-in, if available. For speed we do a single try, which allows a smattering of composites in, but this does not appear to damage the algorithm much, and the correct answer appears to be within a few candidates of the end all the time. We keep the last few hundred candidates, sort them into descending order, then pick the largest that passes Miller-Rabin with 100 tries.



I believe the composites don't hurt the algorithm because they do not confer any selective advantage on their "children", so their average length is no longer than the correct solution. In addition, the candidates in the finalist set all appear to be largely independent of each other, so any given composite tends to contribute only a single candidate, if any. I have no proof of this; I prefer my math to have more vigor than rigor. <tt>:-)</tt>

```perl
for 3..* -> $base {
    say "Starting base $base...";
    my @stems = grep { is-prime($_, 100)}, ^$base;
    my @runoff;
    for 1 .. * -> $digits {
      print ' ', @stems.elems;
      my @new;
      my $place = $base ** $digits;
      my $tries = 1;
      for @stems -> $stem {
	for 1 ..^ $base -> $digit {
	  my $new = $digit * $place + $stem;
	  @new.push($new) if is-prime($new, $tries);
        }
      }
      last unless @new;
      push @runoff, @stems if @new < @stems and @new < 100;
      @stems = @new;
    }
    push @runoff, @stems;
    say "\n  Finalists: ", +@runoff;
 
    for @runoff.sort(-*) -> $finalist {
	my $b = $finalist.base($base);
	say "  Checking :$base\<", $b, '>';
	my $ok = True;
	for $base ** 2, $base ** 3, $base ** 4 ... $base ** $b.chars -> $place {
	    my $f = $finalist % $place;
	    if not is-prime($f, 100) {
		say "    Oops, :$base\<", $f.base($base), '> is not a prime!!!';
		$ok = False;
		last;
	    }
	}
	next unless $ok;
 
	say "  Largest ltp in base $base = $finalist";
	last;
    }
}
```

#### Output:
```
Starting base 3...
 1 1 1
  Finalists: 1
  Checking :3<212>
  Largest ltp in base 3 = 23
Starting base 4...
 2 3 5 5 6 4 2
  Finalists: 12
  Checking :4<3323233>
    Oops, :4<33> is not a prime!!!
  Checking :4<1323233>
    Oops, :4<33> is not a prime!!!
  Checking :4<333323>
  Largest ltp in base 4 = 4091
Starting base 5...
 2 4 4 3 1 1
  Finalists: 8
  Checking :5<222232>
  Largest ltp in base 5 = 7817
Starting base 6...
 3 4 12 26 45 56 65 67 66 57 40 25 14 9 4 2 1
  Finalists: 285
  Checking :6<14141511414451435>
  Largest ltp in base 6 = 4836525320399
Starting base 7...
 3 6 6 4 1 1 1
  Finalists: 11
  Checking :7<6642623>
  Largest ltp in base 7 = 817337
Starting base 8...
 4 12 29 50 67 79 65 52 39 28 17 8 3 2 1
  Finalists: 294
  Checking :8<313636165537775>
  Largest ltp in base 8 = 14005650767869
Starting base 9...
 4 9 15 17 24 16 9 6 5 3
  Finalists: 63
  Checking :9<4284484465>
  Largest ltp in base 9 = 1676456897
Starting base 10...
 4 11 40 101 197 335 439 536 556 528 456 358 278 212 117 72 42 24 13 6 5 4 3 1
  Finalists: 287
  Checking :10<357686312646216567629137>
  Largest ltp in base 10 = 357686312646216567629137
Starting base 11...
 4 8 15 18 15 8 4 2 1
  Finalists: 48
  Checking :11<A68822827>
  Largest ltp in base 11 = 2276005673
Starting base 12...
 5 24 124 431 1179 2616 4948 8054 11732 15460 18100 19546 19777 18280 15771 12574 9636 6866 4625 2990 1867 1152 627 357 189 107 50 20 9 3 1 1
  Finalists: 190
  Checking :12<471A34A164259BA16B324AB8A32B7817>
  Largest ltp in base 12 = 13092430647736190817303130065827539
Starting base 13...
 5 13 21 23 17 11 7 4
  Finalists: 62
  Checking :13<CC4C8C65>
  Largest ltp in base 13 = 812751503
Starting base 14...
 6 28 103 308 694 1329 2148 3089 3844 4290 4311 3923 3290 2609 1948 1298 809 529 332 167 97 55 27 13 5 2
  Finalists: 366
  Checking :14<D967CCD63388522619883A7D23>
  Largest ltp in base 14 = 615419590422100474355767356763
Starting base 15...
 6 22 80 213 401 658 955 1208 1307 1209 1075 841 626 434 268 144 75 46 27 14 7 1
  Finalists: 314
  Checking :15<6C6C2CE2CEEEA4826E642B>
  Largest ltp in base 15 = 34068645705927662447286191
Starting base 16...
 6 32 131 355 792 1369 2093 2862 3405 3693 3519 3140 2660 1930 1342 910 571 335 180 95 50 25 9 5 1
  Finalists: 365
  Checking :16<DBC7FBA24FE6AEC462ABF63B3>
  Largest ltp in base 16 = 1088303707153521644968345559987
Starting base 17...
 6 23 47 64 80 62 43 32 23 8 1
  Finalists: 249
  Checking :17<6C66CC4CC83>
  Largest ltp in base 17 = 13563641583101
Starting base 18...
 7 49 311 1396 5117 15243 38818 85684 167133 293518 468456 680171 911723 1133959 1313343 1423597 1449405 1395514 1270222 1097353 902477 707896 529887 381239 263275 174684 111046 67969 40704 23201 12793 6722 3444 1714 859 422 205 98 39 14 7 1 1
  Finalists: 364
  Checking :18<AF93E41A586HE75A7HHAAB7HE12FG79992GA7741B3D>
  Largest ltp in base 18 = 571933398724668544269594979167602382822769202133808087
Starting base 19...
 7 29 61 106 122 117 93 66 36 18 10 10 6 4
  Finalists: 350
  Checking :19<CIEG86GCEA2C6H>
  Largest ltp in base 19 = 546207129080421139
Starting base 20...
 8 56 321 1311 4156 10963 24589 47737 83011 129098 181707 234685 278792 306852 315113 302684 273080 233070 188331 145016 105557 73276 48819 31244 19237 11209 6209 3383 1689 917 430 196 80 44 20 7 2
  Finalists: 349
  Checking :20<FC777G3CG1FIDI9I31IE5FFB379C7A3F6EFID>
  Largest ltp in base 20 = 1073289911449776273800623217566610940096241078373
Starting base 21...
 8 41 165 457 1079 2072 3316 4727 6003 6801 7051 6732 5862 4721 3505 2474 1662 1039 628 369 219 112 52 17 13 4 2
  Finalists: 200
  Checking :21<G8AGG2GCA8CAK4K68GEA4G2K22H>
  Largest ltp in base 21 = 391461911766647707547123429659688417
Starting base 22...
 8 48 261 1035 3259 8247 17727 33303 55355 82380 111919 137859 158048 167447 165789 153003 132516 108086 83974 61950 43453 29212 18493 11352 6693 3738 2053 1125 594 293 145 70 31 13 6 2 1
  Finalists: 268
  Checking :22<FFHALC8JFB9JKA2AH9FAB4I9L5I9L3GF8D5L5>
  Largest ltp in base 22 = 33389741556593821170176571348673618833349516314271
Starting base 23...
 8 30 78 137 181 200 186 171 121 100 67 41 24 16 9 2 1
  Finalists: 260
  Checking :23<IMMGM6C6IMCI66A4H>
  Largest ltp in base 23 = 116516557991412919458949
```