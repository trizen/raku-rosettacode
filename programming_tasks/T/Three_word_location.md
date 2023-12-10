[1]: https://rosettacode.org/wiki/Three_word_location

# [Three word location][1]

### The task



In large part due to the complete lack of specification, reference implementation, or guidance from the task creator, came up with my own bespoke synthetic word list.



Words always consist of a series of consonant/vowel pairs. Uses a cut down alphabet to reduce possible confusion from overlapping pronunciation.



Some letters with overlapping pronunciation are removed: c: confusable with k or s, g: overlaps with j, x: overlaps with z, q: just because, v: similar to w and we have way more than enough characters anyway.



As it is, with this alphabet we can form 512000 different 6 character "words"; 28126 is a drop in the bucket. To spread out the the words a bit, add a bit of randomness. 28126 fits into 512000 18 and a bit times. Add a random multiple of 28126 to the encoder then modulus it back out on decode. Will get different results on different runs.



We don't bother to pre-calculate and store the words, just generate them on the fly.



Official pronunciation guide:

```perl
# SYNTHETICS HANDLING
my @synth = flat < b d f h j k l m n p r s t w y z > X~ < a e i o u >;
my %htnys = @synth.antipairs;
my $exp   = @synth.elems;

sub synth (Int $v) { @synth[($v + (^18).pick * 28126).polymod($exp xx *).reverse || 0].join }

sub thnys (Str $v) { (sum %htnys{$v.comb(2).reverse} Z* 1, $exp, $exp**2) % 28126 }


# ENCODE / DECODE
sub w-encode ( Rat(Real) $lat, Rat(Real) $lon, :&f = &synth ) {
    $_ = (($lat +  90) * 10000).round.fmt('%021b') ~ (($lon + 180) * 10000).round.fmt('%022b');
    (:2(.substr(0,15)), :2(.substr(15,14)),:2(.substr(29)))».&f
}

sub w-decode ( *@words, :&f = &thnys ) {
    my $bin = (@words».&f Z, <0 1 1>).map({.[0].fmt('%015b').substr(.[1])}).join;
    (:2($bin.substr(0,21))/10000) - 90, (:2($bin.substr(21))/10000) - 180
}


# TESTING
for 51.4337,  -0.2141, # Wimbledon
    21.2596,-157.8117, # Diamond Head crater
   -55.9652, -67.2256, # Monumento Cabo De Hornos
    59.3586,  24.7447, # Lake Raku
    29.2021,  81.5324, # Village Raku
    -7.1662,  53.9470, # The Indian ocean, south west of Seychelles
    28.3852, -81.5638  # Walt Disney World
  -> $lat, $lon {
    my @words = w-encode $lat, $lon;
    my @index = w-encode $lat, $lon, :f( { $_ } );
    printf "Coordinates: %s, %s\n   To Index: %s\n  To 3-word: %s\nFrom 3-word: %s, %s\n From Index: %s, %s\n\n",
      $lat, $lon, @index.Str, @words.Str, w-decode(@words), w-decode @index, :f( { $_ } );
}
```

#### Output:
```
Coordinates: 51.4337, -0.2141
   To Index: 22099 365 12003
  To 3-word: zofobe fohujo habute
From 3-word: 51.4337, -0.2141
 From Index: 51.4337, -0.2141

Coordinates: 21.2596, -157.8117
   To Index: 17384 5133 8891
  To 3-word: nijemo zanaza fupawu
From 3-word: 21.2596, -157.8117
 From Index: 21.2596, -157.8117

Coordinates: -55.9652, -67.2256
   To Index: 5317 15428 13632
  To 3-word: zanohu julaso husese
From 3-word: -55.9652, -67.2256
 From Index: -55.9652, -67.2256

Coordinates: 59.3586, 24.7447
   To Index: 23337 4732 15831
  To 3-word: kapupi hokame supoku
From 3-word: 59.3586, 24.7447
 From Index: 59.3586, 24.7447

Coordinates: 29.2021, 81.5324
   To Index: 18625 5535 10268
  To 3-word: dijule nutuza nefini
From 3-word: 29.2021, 81.5324
 From Index: 29.2021, 81.5324

Coordinates: -7.1662, 53.947
   To Index: 12942 12942 12942
  To 3-word: rakudo rakudo rakudo
From 3-word: -7.1662, 53.947
 From Index: -7.1662, 53.947

Coordinates: 28.3852, -81.5638
   To Index: 18497 11324 1322
  To 3-word: tabesa nekaso bupodo
From 3-word: 28.3852, -81.5638
 From Index: 28.3852, -81.5638
```


(Ok, I admit I manipulated that second to last one, but it **is** a correct and valid 3-word location in this implementation. There is less than 1 chance in 5000 that it will produce that specific word group though.)



### A thought experiment



A little thought experiment... Latitude, longitude to four decimal places is accurate to about 11.1 meters at the equator, smaller the further from the equator you get. 
What would it take to support five decimal places? (Accurate to 1.11 meters.)



`360 * 100000 == 36000000;
ceiling 36000000.log(2) == 26;`



So we need 26 bits to cover 360.00000; half of that for 180.00000, or `26 bits + 25 bits == 51 bits`. `51 / 3 == 17`. `2**17 == 131072` indices. The previous synthetics routine provides much more than enough.



How many sylabics will we need to minimally cover it?



`∛131072 == 50.7968...`



So at least 51. The synthetics routine provide sylabics in blocks of 5, so we would need
at least 11 consonants.



Capriciously and somewhat randomly cutting down the list we arrive at this.



10 times better accuracy in the same three, 6-letter word space.

```perl
# SYNTHETICS HANDLING
my @synth = flat < b d f j k n p r s t w > X~ < a e i o u >;
my %htnys = @synth.antipairs;
my $exp   = @synth.elems;
my $prec  = 100_000;


sub synth (Int $v) { @synth[$v.polymod($exp xx *).reverse || 0].join }

sub thnys (Str $v) { sum %htnys{$v.comb(2).reverse} Z× 1, $exp, $exp² }


# ENCODE / DECODE
sub w-encode ( Rat(Real) $lat, Rat(Real) $lon, :&f = &synth ) {
    $_ = (($lat +  90) × $prec).round.fmt('%025b') ~ (($lon + 180) × $prec).round.fmt('%026b');
    (:2(.substr(0,17)), :2(.substr(17,17)), :2(.substr(34)))».&f
}

sub w-decode ( *@words, :&f = &thnys ) {
    my $bin = @words».&f.map({.fmt('%017b')}).join;
    (:2($bin.substr(0,25))/$prec) - 90, (:2($bin.substr(25))/$prec) - 180
}


# TESTING
for 51.43372,  -0.21412, # Wimbledon center court
    21.25976,-157.81173, # Diamond Head summit
   -55.96525, -67.22557, # Monumento Cabo De Hornos
    28.3852,  -81.5638,  # Walt Disney World
    89.99999, 179.99999, # test some
   -89.99999,-179.99999  # extremes
  -> $lat, $lon {
    my @words = w-encode $lat, $lon;
    printf "Coordinates: %s, %s\n   To Index: %s\n  To 3-word: %s\nFrom 3-word: %s, %s\n\n",
      $lat, $lon, w-encode($lat, $lon, :f({$_})).Str, @words.Str, w-decode(@words);
}
```

#### Output:
```
Coordinates: 51.43372, -0.21412
   To Index: 55247 71817 21724
  To 3-word: jofuni kosasi diduwu
From 3-word: 51.43372, -0.21412

Coordinates: 21.25976, -157.81173
   To Index: 43460 110608 121675
  To 3-word: fukafa repebo safija
From 3-word: 21.25976, -157.81173

Coordinates: -55.96525, -67.22557
   To Index: 13294 108118 5251
  To 3-word: bukeru rasaso besane
From 3-word: -55.96525, -67.22557

Coordinates: 28.3852, -81.5638
   To Index: 46244 28747 13220
  To 3-word: jajasu duniri bukaka
From 3-word: 28.3852, -81.5638

Coordinates: 89.99999, 179.99999
   To Index: 70312 65298 86271
  To 3-word: kofoki kepifo nonope
From 3-word: 89.99999, 179.99999

Coordinates: -89.99999, -179.99999
   To Index: 0 512 1
  To 3-word: ba duji be
From 3-word: -89.99999, -179.99999
```
