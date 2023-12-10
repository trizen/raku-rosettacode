[1]: https://rosettacode.org/wiki/Factorial_base_numbers_indexing_permutations_of_a_collection

# [Factorial base numbers indexing permutations of a collection][1]





Using my interpretation of the task instructions as shown on the [discussion page](https://rosettacode.org/wiki/Talk:Factorial_base_numbers_indexing_permutations_of_a_collection#Mojibake_and_misspellings).

```perl
sub postfix:<!> (Int $n) { (flat 1, [\*] 1..*)[$n] }

multi base (Int $n is copy, 'F', $length? is copy) {
    constant @fact = [\*] 1 .. *;
    my $i = $length // @fact.first: * > $n, :k;
    my $f;
    [ @fact[^$i].reverse.map: { ($n, $f) = $n.polymod($_); $f } ]
}

sub fpermute (@a is copy, *@f) { (^@f).map: { @a[$_ .. $_ + @f[$_]].=rotate(-1) }; @a }

put "Part 1: Generate table";
put $_.&base('F', 3).join('.') ~ ' -> ' ~ [0,1,2,3].&fpermute($_.&base('F', 3)).join for ^24;

put "\nPart 2: Compare 11! to 11! " ~ '¯\_(ツ)_/¯';
# This is kind of a weird request. Since we don't actually need to _generate_
# the permutations, only _count_ them: compare count of 11! vs count of 11!
put "11! === 11! : {11! === 11!}";

put "\nPart 3: Generate the given task shuffles";
my \Ω = <A♠ K♠ Q♠ J♠ 10♠ 9♠ 8♠ 7♠ 6♠ 5♠ 4♠ 3♠ 2♠ A♥ K♥ Q♥ J♥ 10♥ 9♥ 8♥ 7♥ 6♥ 5♥ 4♥ 3♥ 2♥
         A♦ K♦ Q♦ J♦ 10♦ 9♦ 8♦ 7♦ 6♦ 5♦ 4♦ 3♦ 2♦ A♣ K♣ Q♣ J♣ 10♣ 9♣ 8♣ 7♣ 6♣ 5♣ 4♣ 3♣ 2♣
>;

my @books = <
    39.49.7.47.29.30.2.12.10.3.29.37.33.17.12.31.29.34.17.25.2.4.25.4.1.14.20.6.21.18.1.1.1.4.0.5.15.12.4.3.10.10.9.1.6.5.5.3.0.0.0
    51.48.16.22.3.0.19.34.29.1.36.30.12.32.12.29.30.26.14.21.8.12.1.3.10.4.7.17.6.21.8.12.15.15.13.15.7.3.12.11.9.5.5.6.6.3.4.0.3.2.1
>;

put "Original deck:";
put Ω.join;

put "\n$_\n" ~ Ω[(^Ω).&fpermute($_.split: '.')].join for @books;

put "\nPart 4: Generate a random shuffle";
my @shoe = (+Ω … 2).map: { (^$_).pick };
put @shoe.join('.');
put Ω[(^Ω).&fpermute(@shoe)].join;

put "\nSeems to me it would be easier to just say: Ω.pick(*).join";
put Ω.pick(*).join;
```

#### Output:
```
Part 1: Generate table
0.0.0 -> 0123
0.0.1 -> 0132
0.1.0 -> 0213
0.1.1 -> 0231
0.2.0 -> 0312
0.2.1 -> 0321
1.0.0 -> 1023
1.0.1 -> 1032
1.1.0 -> 1203
1.1.1 -> 1230
1.2.0 -> 1302
1.2.1 -> 1320
2.0.0 -> 2013
2.0.1 -> 2031
2.1.0 -> 2103
2.1.1 -> 2130
2.2.0 -> 2301
2.2.1 -> 2310
3.0.0 -> 3012
3.0.1 -> 3021
3.1.0 -> 3102
3.1.1 -> 3120
3.2.0 -> 3201
3.2.1 -> 3210

Part 2: Compare 11! to 11! ¯\_(ツ)_/¯
11! === 11! : True

Part 3: Generate the given task shuffles
Original deck:
A♠K♠Q♠J♠10♠9♠8♠7♠6♠5♠4♠3♠2♠A♥K♥Q♥J♥10♥9♥8♥7♥6♥5♥4♥3♥2♥A♦K♦Q♦J♦10♦9♦8♦7♦6♦5♦4♦3♦2♦A♣K♣Q♣J♣10♣9♣8♣7♣6♣5♣4♣3♣2♣

39.49.7.47.29.30.2.12.10.3.29.37.33.17.12.31.29.34.17.25.2.4.25.4.1.14.20.6.21.18.1.1.1.4.0.5.15.12.4.3.10.10.9.1.6.5.5.3.0.0.0
A♣3♣7♠4♣10♦8♦Q♠K♥2♠10♠4♦7♣J♣5♥10♥10♣K♣2♣3♥5♦J♠6♠Q♣5♠K♠A♦3♦Q♥8♣6♦9♠8♠4♠9♥A♠6♥5♣2♦7♥8♥9♣6♣7♦A♥J♦Q♦9♦2♥3♠J♥4♥K♦

51.48.16.22.3.0.19.34.29.1.36.30.12.32.12.29.30.26.14.21.8.12.1.3.10.4.7.17.6.21.8.12.15.15.13.15.7.3.12.11.9.5.5.6.6.3.4.0.3.2.1
2♣5♣J♥4♥J♠A♠5♥A♣6♦Q♠9♣3♦Q♥J♣10♥K♣10♣5♦7♥10♦3♠8♥10♠7♠6♥5♠K♥4♦A♥4♣2♥9♦Q♣8♣7♦6♣3♥6♠7♣2♦J♦9♥A♦Q♦8♦4♠K♦K♠3♣2♠8♠9♠

Part 4: Generate a random shuffle
47.9.46.16.28.8.36.27.29.1.9.27.1.16.21.22.28.34.30.8.19.27.18.22.3.25.15.20.12.14.8.9.11.1.4.0.3.5.4.2.2.10.8.1.6.1.2.4.1.2.1
6♣5♠5♣10♥10♦6♠K♣9♦6♦K♠2♠5♦Q♠5♥Q♦8♦J♣2♣8♣A♥K♦9♣A♦2♦9♠4♣3♥A♣7♥2♥Q♥9♥4♥J♠4♠A♠3♠8♥J♥7♠K♥3♣10♣8♠Q♣6♥7♦7♣J♦3♦4♦10♠

Seems to me it would be easier to just say: Ω.pick(*).join
5♦3♠8♦10♦2♥7♠7♦Q♦A♠5♣8♣Q♠4♠2♦K♦5♠Q♥7♣10♠2♠K♠J♣9♣3♣4♥3♥4♦3♦Q♣2♣4♣J♦9♠A♣J♠10♣6♣9♦6♠10♥6♥9♥J♥7♥K♥A♦8♠A♥5♥8♥K♣6♦
```
