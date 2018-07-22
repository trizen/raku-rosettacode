[1]: https://rosettacode.org/wiki/Find_largest_left_truncatable_prime_in_a_given_base

# [Find largest left truncatable prime in a given base][1]

Pretty fast for bases 3 .. 11. 12 is slow. 18 is glacial.

```perl
for 3 .. * -> $base {
    say "Starting base $base...";
    my @stems = grep { .is-prime }, ^$base;
    for 1 .. * -> $digits {
        print ' ', @stems.elems;
        my @new;
        my $place = $base ** $digits;
        for 1 ..^ $base -> $digit {
            my $left = $digit * $place;
            for @stems -> $stem {
	        my $new = $left + $stem;
	        @new.push($new) if $new.is-prime;
            }
        }
        last unless @new;
        @stems = @new;
    }
    say "\nLargest ltp in base $base = { @stems.tail } or :$base\<{@stems.tail.base($base)}>\n";
}
```

#### Output:
```
Starting base 3...
 1 1 1
Largest ltp in base 3 = 23 or :3<212>

Starting base 4...
 2 2 3 3 3 3
Largest ltp in base 4 = 4091 or :4<333323>

Starting base 5...
 2 4 4 3 1 1
Largest ltp in base 5 = 7817 or :5<222232>

Starting base 6...
 3 4 12 25 44 54 60 62 59 51 35 20 12 7 3 2 1
Largest ltp in base 6 = 4836525320399 or :6<14141511414451435>

Starting base 7...
 3 6 6 4 1 1 1
Largest ltp in base 7 = 817337 or :7<6642623>

Starting base 8...
 4 12 29 50 66 77 61 51 38 27 17 8 3 2 1
Largest ltp in base 8 = 14005650767869 or :8<313636165537775>

Starting base 9...
 4 9 15 17 24 16 9 6 5 3
Largest ltp in base 9 = 1676456897 or :9<4284484465>

Starting base 10...
 4 11 39 99 192 326 429 521 545 517 448 354 276 212 117 72 42 24 13 6 5 4 3 1
Largest ltp in base 10 = 357686312646216567629137 or :10<357686312646216567629137>

Starting base 11...
 4 8 15 18 15 8 4 2 1
Largest ltp in base 11 = 2276005673 or :11<A68822827>

Starting base 12...
 5 23 119 409 1124 2496 4733 7711 11231 14826 17341 18787 19001 17567 15169 12085 9272 6606 4451 2882 1796 1108 601 346 181 103 49 19 8 2 1 1
Largest ltp in base 12 = 13092430647736190817303130065827539 or :12<471A34A164259BA16B324AB8A32B7817>

Starting base 13...
 5 13 20 23 17 11 7 4
Largest ltp in base 13 = 812751503 or :13<CC4C8C65>

Starting base 14...
 6 26 101 300 678 1299 2093 3017 3751 4196 4197 3823 3206 2549 1908 1269 783 507 322 163 97 55 27 13 5 2
Largest ltp in base 14 = 615419590422100474355767356763 or :14<D967CCD63388522619883A7D23>

Starting base 15...
 6 22 79 207 391 644 934 1177 1275 1167 1039 816 608 424 261 142 74 45 25 13 7 1
Largest ltp in base 15 = 34068645705927662447286191 or :15<6C6C2CE2CEEEA4826E642B>

Starting base 16...
 6 31 124 337 749 1292 1973 2695 3210 3490 3335 2980 2525 1840 1278 878 556 326 174 93 50 25 9 5 1
Largest ltp in base 16 = 1088303707153521644968345559987 or :16<DBC7FBA24FE6AEC462ABF63B3>

Starting base 17...
 6 22 43 55 74 58 41 31 23 8 1
Largest ltp in base 17 = 13563641583101 or :17<6C66CC4CC83>
...
```