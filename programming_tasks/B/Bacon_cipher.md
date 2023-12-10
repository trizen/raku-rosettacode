[1]: https://rosettacode.org/wiki/Bacon_cipher

# [Bacon cipher][1]





### alternative steganography algorithm



Not truly a Bacon Cipher as it doesn't encode using font variations. But fits with the spirit if not the exact definition.

```perl
my $secret = q:to/END/;
    This task is to implement a program for encryption and decryption
    of plaintext using the simple alphabet of the Baconian cipher or
    some other kind of representation of this alphabet (make anything
    signify anything). This example will work with anything in the
    ASCII range... even code! $r%_-^&*(){}+~ #=`/\';*1234567890"'
    END

my $text = q:to/END/;
    Bah. It isn't really practical to use typeface changes to encode
    information, it is too easy to tell that there is something going
    on and will attract attention. Font changes with enough regularity
    to encode mesages relatively efficiently would need to happen so
    often it would be obvious that there was some kind of manipulation
    going on. Steganographic encryption where it is obvious that there
    has been some tampering with the carrier is not going to be very
    effective. Not that any of these implementations would hold up to
    serious scrutiny anyway. Anyway, here's a semi-bogus implementation
    that hides information in white space. The message is hidden in this
    paragraph of text. Yes, really. It requires a fairly modern file
    viewer to display (not display?) the hidden message, but that isn't
    too unlikely any more. It may be stretching things to call this a
    Bacon cipher, but I think it falls within the spirit of the task,
    if not the exact definition.
    END
#'
my @enc = "﻿", "​";
my %dec = @enc.pairs.invert;

sub encode ($c) { @enc[($c.ord).fmt("%07b").comb].join('') }

sub hide ($msg is copy, $text) { 
    $msg ~= @enc[0] x (0 - ($msg.chars % 8)).abs;
    my $head = $text.substr(0,$msg.chars div 8);
    my $tail = $text.substr($msg.chars div 8, *-1);
    ($head.comb «~» $msg.comb(/. ** 8/)).join('') ~ $tail;
}

sub reveal ($steg) {
    join '', map { :2(%dec{$_.comb}.join('')).chr }, 
    $steg.subst( /\w | <punct> | " " | "\n" /, '', :g).comb(/. ** 7/);
}

my $hidden = join '', map  { .&encode }, $secret.comb;

my $steganography = hide $hidden, $text;

say "Steganograpic message hidden in text:";
say $steganography;

say '*' x 70;

say "Hidden message revealed:";
say reveal $steganography;
```

#### Output:
```
Steganograpic message hidden in text:
B​﻿​﻿​﻿﻿​a​﻿​﻿﻿﻿​​h﻿​﻿﻿​​​​.﻿﻿​​﻿​﻿﻿ ﻿﻿﻿​​​﻿​I﻿﻿​​﻿﻿﻿﻿t​​​​﻿﻿​​ ​​﻿​﻿​​﻿i​﻿﻿﻿﻿﻿​​s﻿​﻿﻿​​​​n﻿﻿​​﻿​﻿﻿'﻿﻿﻿​​​﻿​t﻿﻿​​﻿​​​ ​﻿​﻿﻿﻿﻿﻿r​​﻿​﻿﻿​​e​﻿​​﻿​​​a​﻿﻿﻿﻿​​﻿l​​﻿﻿​​﻿﻿l​﻿​​​﻿​​y﻿​​​﻿﻿​﻿ ​​​﻿​​​﻿p​​​﻿​﻿﻿﻿r​﻿﻿﻿﻿﻿​​a﻿﻿﻿﻿​﻿​﻿c﻿﻿﻿﻿​​​﻿t﻿﻿﻿​​​﻿﻿i​﻿​​﻿​​​c​​​﻿﻿​​​a​​​﻿﻿​﻿​l​﻿﻿﻿﻿​​​ ﻿​​﻿​﻿​﻿t﻿﻿﻿﻿​​﻿﻿o​​﻿​​﻿​​ ​​​​​﻿﻿​u﻿﻿​﻿﻿﻿﻿﻿s​​﻿﻿​﻿​​e​﻿​​​﻿​​ ﻿﻿﻿​​​​​t﻿﻿​﻿​​​​y﻿﻿​​​​﻿﻿p﻿﻿​​​﻿​﻿e﻿​​﻿​﻿﻿​f​​﻿​​​​​a​﻿​​​﻿﻿​c﻿﻿﻿﻿﻿​​﻿e﻿﻿﻿​​​﻿​ ​​﻿​​﻿﻿​c﻿﻿﻿​﻿﻿﻿﻿h﻿​​﻿﻿​﻿﻿a​​﻿﻿​﻿​​n​﻿﻿﻿​​​​g​﻿﻿​﻿​​​e​﻿﻿​​​​﻿s﻿﻿﻿​​​﻿​ ﻿﻿​​﻿​﻿﻿t​​​﻿​​​​o​​﻿​​​﻿﻿ ﻿﻿​﻿​﻿​​e﻿​​​​​​﻿n﻿​​﻿﻿​﻿﻿c﻿﻿﻿​​​﻿﻿o﻿﻿​​﻿​​﻿d﻿​​﻿﻿﻿﻿​e​​﻿​﻿﻿​​
i​﻿​﻿﻿​​﻿n﻿​﻿​​​​​f﻿﻿﻿​​​﻿​o﻿﻿﻿​﻿﻿﻿﻿r﻿​​​﻿​﻿​m​​​﻿﻿​​​a​﻿​﻿﻿​​​t﻿​​​﻿​​﻿i﻿​​​﻿​﻿﻿o﻿﻿﻿​​​﻿​n﻿﻿​​﻿​﻿﻿,﻿​​﻿﻿​﻿​ ﻿​﻿﻿﻿﻿﻿​i​​﻿﻿​​​​t﻿​﻿﻿​​​﻿ ​​﻿​​​​﻿i﻿﻿﻿​​﻿​​s﻿﻿​​﻿﻿​﻿ ​﻿​﻿﻿﻿﻿﻿t​​﻿﻿﻿﻿​​o​﻿​​﻿﻿​​o​﻿﻿﻿﻿​​﻿ ​﻿﻿﻿​​﻿﻿e﻿﻿​​​﻿﻿﻿a​﻿​​﻿﻿​﻿s​​​​﻿​﻿﻿y﻿​﻿﻿﻿﻿﻿​ ​﻿​​​​​​t﻿﻿​​﻿﻿​﻿o﻿﻿﻿﻿​​​﻿ ​﻿﻿​​﻿​﻿t﻿﻿​​﻿﻿​﻿e​﻿​﻿﻿﻿﻿﻿l​﻿﻿﻿﻿​﻿​l​﻿﻿﻿﻿​​​ ﻿﻿﻿​​​​﻿t​​​​​​﻿​h​​﻿​​﻿​﻿a﻿​​​﻿﻿﻿﻿t​​​﻿​​​﻿ ﻿​﻿﻿﻿﻿﻿​t​﻿﻿﻿​​​​h﻿​﻿﻿​​​​e﻿﻿﻿﻿​​﻿​r﻿﻿﻿​​﻿﻿​e﻿​​​​﻿﻿​ ﻿﻿​﻿﻿﻿﻿﻿i​​﻿​​​​​s​​﻿﻿​﻿﻿﻿ ﻿​﻿​﻿​​​s﻿﻿​​​​﻿​o​​​​​﻿​​m﻿​​​﻿﻿​﻿e​﻿​﻿﻿﻿﻿﻿t​​﻿​​​​​h​​﻿​﻿﻿​​i﻿​﻿﻿﻿​​﻿n﻿​﻿​​​​﻿g﻿​﻿﻿​﻿﻿﻿ ﻿﻿​​﻿​﻿​g​​​﻿​﻿﻿​o​​﻿​​​﻿​i​﻿﻿​﻿﻿﻿​n﻿﻿﻿﻿﻿​​﻿g​​​​​​﻿﻿
o﻿﻿​​​﻿﻿​n﻿​​﻿﻿​﻿​ ​​​﻿﻿﻿﻿​a​​﻿﻿​﻿​​n﻿﻿​﻿​​​​d﻿﻿​​​​﻿﻿ ​﻿​​​﻿​​w​﻿​​​﻿​﻿i﻿​​﻿﻿﻿﻿​l​​​﻿​﻿﻿​l​﻿​﻿﻿​​​ ﻿​​​​​​﻿a​​​﻿﻿​﻿﻿t﻿﻿﻿​​﻿​​t​​​​﻿﻿​​r﻿﻿​﻿﻿﻿﻿﻿a​​​﻿​﻿﻿​c​﻿​﻿﻿﻿​​t﻿​﻿﻿​​​​ ﻿﻿​​﻿​﻿﻿a﻿﻿﻿​​﻿﻿﻿t﻿​​​﻿​​﻿t﻿​​​﻿﻿﻿﻿e​​﻿​﻿﻿﻿​n​﻿﻿﻿﻿​​​t﻿﻿﻿​﻿​​﻿i﻿​﻿​​​​﻿o​﻿﻿﻿​﻿﻿﻿n﻿﻿﻿​﻿​﻿﻿.﻿​​﻿​​﻿​ ​​﻿﻿﻿﻿​​F​﻿​﻿​​​​o﻿﻿​﻿​﻿​﻿n﻿﻿﻿﻿​​﻿﻿t﻿﻿​​​﻿​​ ​﻿​​​​﻿﻿c​​​​﻿​﻿﻿h​​﻿​﻿﻿﻿​a​﻿​﻿﻿​​​n﻿​​​﻿​​﻿g﻿​​​﻿﻿﻿​e﻿​﻿​​​﻿﻿s​​​​﻿​﻿﻿ ​​​﻿﻿​​​w​​﻿​​​﻿​i​﻿​﻿﻿​​​t﻿﻿​​﻿​​​h​﻿﻿​﻿​﻿﻿ ﻿﻿﻿​​﻿﻿﻿e﻿​​​﻿​​​n﻿​​​​﻿﻿​o​​​﻿​﻿﻿​u​﻿​﻿﻿﻿​​g﻿​﻿﻿​​​﻿h​​​﻿​​﻿﻿ ​​​﻿​﻿​﻿r﻿​﻿​﻿​​​e﻿﻿​﻿﻿﻿﻿﻿g​﻿​﻿​﻿﻿​u​﻿​﻿﻿﻿​​l﻿​﻿﻿​​​​a﻿﻿​​﻿​﻿﻿r﻿﻿﻿​​﻿﻿​i﻿​​​​​﻿﻿t﻿​​﻿﻿﻿﻿​y​​﻿​​﻿​​
t﻿​​﻿﻿​​﻿o﻿​﻿​﻿​﻿﻿ ﻿﻿﻿​​​﻿​e​​​​﻿​﻿﻿n​​​﻿​​﻿﻿c​​﻿​​﻿﻿﻿o​﻿﻿﻿﻿﻿​​d​﻿​​​​​﻿e​​​​​​​﻿ ﻿​﻿​​﻿​﻿m​​﻿​﻿﻿﻿﻿e﻿​​​﻿​​​s​​﻿​﻿﻿​​a​​﻿​﻿﻿​​g﻿​﻿﻿﻿﻿​﻿e﻿﻿﻿﻿​​﻿﻿s﻿﻿​​​﻿​​ ​﻿​​​​﻿﻿r​​​​﻿​﻿﻿e​​﻿​﻿﻿﻿​l​﻿​﻿﻿​​​a﻿​​​﻿​​﻿t﻿​​​﻿​﻿﻿i﻿﻿﻿​​﻿​﻿v﻿​​​﻿​​​e﻿﻿​﻿﻿﻿﻿﻿l​​​﻿​﻿﻿​y​﻿​﻿﻿﻿​​ ﻿﻿​﻿​﻿﻿﻿e​﻿​﻿​﻿﻿﻿f﻿﻿​​﻿​﻿﻿f​​​﻿﻿﻿﻿​i​​﻿﻿​﻿﻿​c​﻿﻿​﻿﻿​﻿i​﻿﻿﻿﻿﻿​​e​﻿﻿​﻿​​﻿n﻿﻿﻿​​​﻿​t​​﻿​​﻿﻿​l​​​​﻿﻿​﻿y​﻿​﻿​​​﻿ ﻿​﻿​​​﻿﻿w​﻿​​​﻿﻿​o﻿﻿﻿﻿﻿​​﻿u﻿​﻿​​​​﻿l​​﻿​​﻿﻿​d﻿​​​﻿​​​ ﻿﻿​﻿﻿﻿﻿﻿n​​﻿﻿﻿​​​e​﻿​​​​​​e﻿﻿​﻿﻿​​﻿d﻿​﻿​﻿​﻿﻿ ﻿﻿​﻿​﻿﻿﻿t﻿﻿﻿​﻿﻿​﻿o﻿​​​﻿﻿​﻿ ﻿​﻿﻿​﻿​​h﻿​​​​​﻿​a﻿​​﻿​​﻿​p​​​﻿﻿​﻿﻿p​​﻿﻿​﻿​﻿e​﻿﻿​﻿​﻿﻿n﻿﻿​﻿​﻿﻿​ ​​​​﻿​​​s​​​​﻿​﻿​o﻿​﻿​​​​​
o﻿﻿﻿﻿​﻿﻿﻿f​​﻿​​​​﻿t​​​﻿﻿﻿﻿﻿e﻿​﻿​​​​​n﻿​​​﻿﻿﻿​ ﻿﻿​​​﻿​​i​﻿​​﻿​﻿​t﻿​﻿﻿​​﻿﻿ ﻿​﻿​​﻿﻿​w﻿﻿​​﻿﻿​​o﻿​​﻿​﻿﻿﻿u​​﻿​﻿​﻿​l​﻿​​﻿﻿​​d﻿​​​﻿​​​ ﻿﻿﻿﻿​​​﻿b﻿​﻿​​﻿﻿﻿e﻿﻿​﻿﻿﻿​﻿ ﻿​﻿﻿​​​﻿o﻿﻿​﻿​﻿﻿﻿bvious that there was some kind of manipulation
going on. Steganographic encryption where it is obvious that there
has been some tampering with the carrier is not going to be very
effective. Not that any of these implementations would hold up to
serious scrutiny anyway. Anyway, here's a semi-bogus implementation
that hides information in white space. The message is hidden in this
paragraph of text. Yes, really. It requires a fairly modern file
viewer to display (not display?) the hidden message, but that isn't
too unlikely any more. It may be stretching things to call this a
Bacon cipher, but I think it falls within the spirit of the task,
if not the exact definition.
**********************************************************************
Hidden message revealed:
This task is to implement a program for encryption and decryption
of plaintext using the simple alphabet of the Baconian cipher or
some other kind of representation of this alphabet (make anything
signify anything). This example will work with anything in the
ASCII range... even code! $r%_-^&*(){}+~ #=`/\';*1234567890"'
```


### Bacon cipher solution

```perl
my @abc = 'a' .. 'z';
my @symb = ' ', |@abc;  # modified Baconian charset - space and full alphabet
# TODO original one with I=J U=V, nice for Latin

my %bacon = @symb Z=> (^@symb).map(*.base(2).fmt("%05s"));
my %nocab = %bacon.antipairs;

sub bacon ($s, $msg) {
  my @raw = $s.lc.comb;
  my @txt := @raw[ (^@raw).grep({@raw[$_] (elem) @abc}) ];
  for $msg.lc.comb Z @txt.batch(5) -> ($c-msg, @a) {
    for @a.kv -> $i, $c-str {
      (my $x := @a[$i]) = $x.uc if %bacon{$c-msg}.comb[$i].Int.Bool;
  } }
  @raw.join;
}

sub unbacon ($s) {
  my $chunks = ($s ~~ m:g/\w+/)>>.Str.join.comb(/.**5/);
  $chunks.map(*.comb.map({.uc eq $_})».Int».Str.join).map({%nocab{$_}}).join;
}

my $msg = "Omnes homines dignitate et iure liberi et pares nascuntur";

my $str = <Lorem ipsum dolor sit amet, consectetur adipiscing elit,
sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi
ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit
in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
Excepteur sint occaecat cupidatat non proident, sunt in culpa
qui officia deserunt mollit anim id est laborum.>;

my $baconed = bacon $str, $msg;
$baconed = $baconed.?naive-word-wrapper || $baconed;
# FIXME ^^^ makes dbl space after .
say "text:\n$baconed\n";
my $unbaconed = unbacon($baconed).trim.uc;
say "hidden message:\n$unbaconed";
```

#### Output:
```
text:
lOREM iPSuM dOLOr siT aMEt, cONsectetUr adiPISCiNG eLiT, seD dO EIusmOd
TEmpOR incididUnt uT laBorE ET dOLOre MagNA aLiqua.  ut ENiM ad miNiM
veniam, qUiS NoStrud exerCitATiOn ULlaMco lAbOris nisI Ut alIquIp ex Ea
coMmODo cOnsEquAt.  duis auTe IRuRe dolor iN reprehenDEriT in vOlUPtaTE
velit eSSE cilluM DolORe eu FUGiAt NuLLA pArIatUr.  ExCEptEur sint
occaecat cupidatat non proident, sunt in culpa qui officia deserunt
mollit anim id est laborum.

hidden message:
OMNES HOMINES DIGNITATE ET IURE LIBERI ET PARES NASCUNTUR
```
